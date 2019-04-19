<template>

<div>
    <div >
        <div id="table" class="table">
            <span class="table-add float-right mb-3 mr-2"><a href="#!" class="text-success"><i class="fa fa-plus fa-2x" aria-hidden="true"></i></a></span>
            <table class="table table-bordered table-responsive-md table-striped text-center" >
               <!-- class="table table-bordered table-responsive-md table-striped text-center"  -->
                <tr>
                    <th class="text-center" v-for="column in columns">
                        {{ column }}
                    </th>
                    <th class="text-center"></th>
                </tr>
                <tr v-for="item in invoice.items">
                    <!-- <td class="pt-3-half"  v-for="column in columns" contenteditable="true" v-model="item[column]"  @input="input(column,item,$event.target.value)">{{item[column]}}</td> -->
                    <td class="pt-3-half"  v-for="column in columns">
                      <input type="text"  v-model="item[column]" @input="input(column,item)" />
                    </td>
                    <td>
                      <a @click="delItem(item)" class="delete" title="Delete"><icon name="trash-alt"></icon></a>
                    </td>
                </tr>
            </table>
        </div>
    </div>

  <div>
    <div  style="float:left">
    <template>
        <v-autocomplete :items="auto_items" v-model="auto_item" :get-label="getLabel" :component-item='template' @update-items="updateAutoItems">
          <div style="clear:both"></div>
        </v-autocomplete>
    </template>
  </div>
    <a  @click.prevent="add"><icon style="height:50px;width:50px;float:left" name="plus"></icon></a>
    <i-button type="primary" style="margin:10px"  @click.prevent="save">Save</i-button>
  </div>
</div>
<!-- Editable table -->
</template>
<script>
import 'bootstrap/dist/css/bootstrap.min.css'
import $ from "jquery";
import 'vue-awesome/icons/trash-alt';
import 'vue-awesome/icons/plus';

import Icon from 'vue-awesome/components/Icon';

import 'v-autocomplete/dist/v-autocomplete.css'
import ItemTemplate from './ItemTemplate.vue'
import Autocomplete from 'v-autocomplete'
import API from '../../../api/config';

  export default {
    name: 'edittable',
    components: {
        Icon,
        ItemTemplate,
        Autocomplete
    },
    props: {
        columns: {
            type: Array,
            required: true,
        },
        // items: {
        //       type: Array,
        //       required: true,
        // },
        invoice: {
               type: Object,
               default: () => ({})
        },
        isCreate:  {
               type: Boolean,
               default:false
        }
    },
    // mounted (){
    //   console.log(this.invoice.items[0]);
    // },
    data () {
         return {
           auto_item: {},
           auto_items: [],
           template: ItemTemplate
         }
    },
    methods: {
        getDigits( n ){
          return Math.round(n * 100) / 100;
        },
        input(column,item){
          if(column === 'quantity' ||column === 'unitAmount')
          {
            item.totalamount = this.getDigits( item.quantity * item.unitAmount  );
            item.totalamounts = ""+ item.totalamount;
            let gstr = 0.1;
            
            // alert(this.invoice.gstRatio);
            if(this.invoice.gstRatio!=0.1)
            {
              gstr = this.invoice.gstRatio;
            }
                
            item.gst = this.getDigits(item.totalamount*gstr);
            
            item.gsts = this.getDigits(item.gst);
            item.subtotal =this.getDigits(item.totalamount +  item.gst );
            item.subtotals = ""+item.subtotal;
            let total =0.00;
            let totalm =0.00;
            this.invoice.items.forEach(function (item, i) {
                total = total +item.subtotal;
                totalm  += totalm + item.totalamount;
            });
            this.invoice.totalamount =  this.getDigits(totalm);
            this.invoice.subtotal = this.getDigits(total);
            this.invoice.subtotals =""+  this.invoice.subtotal;
          }
        },
        keep2digits(num)
        {
          return Math.floor(num * 100) / 100;
        },
    		delItem(item){
          for(var i=0;i<this.invoice.items.length;i++)
          {
              if(this.invoice.items[i].inventoryItemCode == item.inventoryItemCode)
              {
                this.invoice.items.splice(i,1);
                return;
              }
          }
      },
      save(){
        let url =API.host+"/api/invoices/update";
        if(this.isCreate)
        {
          url =API.host+"/api/invoices/add";
        }
        // console.log(this.invoice);
        this.$http.post(url, this.invoice,{  headers: {  'Content-Type': 'application/json'  }  }
            ).then(function(res){
                        // this.data = {};
                         alert("success！");
                          // console.log(res.data.body.message);
              },function(res){
                        alert("fail!");
                             // this.data = {};
                                   // console.log(res.body.message);
              });

      },
      updateAutoItems(text){
        this.$http.get(API.host+"/api/items/query/"+text).then(function(res){
                      if(res.data){
                        this.auto_items = res.data;
                      }
                  },function(res){
                    // this.loading = false;
                       alert("Error: "+res.status);
                 });
      },
      getLabel (item) {
        if(item)
        return item.name;
      },
      add(){
        let item = {};
          // columns:["inventoryItemCode","description","quantity","unitAmount","totalamount"]
        item[this.columns[0]] = this.auto_item.code;
        item[this.columns[1]] = this.auto_item.name;
        item[this.columns[2]] = 0.00;
        item[this.columns[3]] = this.auto_item.price;
        item[this.columns[4]] = 0.00;
        item[this.columns[5]] = "0.00";
        item[this.columns[6]] =  "0.00";
        item[this.columns[7]] = this.auto_item.accountCode;
        this.invoice.items.push(item);
      }
    }
}
</script>
<style lang="stylus">
.v-autocomplete
  .v-autocomplete-input-group
    .v-autocomplete-input
      font-size 1.3em
      padding 1px 2px
      box-shadow none
      border 1px solid #157977
      width 100%

      /*calc(30% - 32px)*/
      outline none
      background-color #eee
    &.v-autocomplete-selected
      .v-autocomplete-input
        color green
        background-color #f2fff2
  .v-autocomplete-list
    width 100%
    text-align left
    border none
    border-top none
    max-height 400px
    overflow-y auto
    border-bottom 1px solid #157977
    .v-autocomplete-list-item
      cursor pointer
      background-color #fff
      padding 10px
      border-bottom 1px solid #157977
      border-left 1px solid #157977
      border-right 1px solid #157977
      &:last-child
        border-bottom none
      &:hover
        background-color #eee
      abbr
        opacity 0.8
        font-size 0.8em
        display block
        font-family sans-serif

pre
  text-align left
  white-space pre-wrap
  background-color #eee
  border 1px solid silver
  padding 20px !important
  border-radius 10px
  font-family monospace !important
.left
  text-align left
.note
  border-left 5px solid #ccc
  padding 10px
</style>
