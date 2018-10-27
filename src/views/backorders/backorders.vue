<template>
  <div>
       <Row>
       <Col span="24">
         <vue-bootstrap-table
           :columns="columns"
           :values="data"
           :show-filter="true"
           :sortable="true"
           :paginated="true"
           :ajax="aj"
           :rowClickHandler="rowClicked"
           :filter-case-sensitive="false"
           :default-order-direction="true"
           @cellDataModifiedEvent="onCellDataModifiedEvent"
           @ajaxLoadedEvent="onAjaxLoadedEvent"
           @ajaxLoadingErro="onAjaxLoadingError"
           >
         </vue-bootstrap-table>
         <v-dialog/>
         <modal name="detail">
          {{this.current.invoiceNumber}}
         </modal>
       </Col>
       </Row>
       <Row>
       <Col span="24" style="margin-top:50px">
         <Form ref="item" :label-width="100">
             
             <FormItem label="invoiceNumber" prop="opinion">
                 <Input :disabled="hasSubmit" v-model="invoiceNumber" type="textarea" :autosize="{minRows: 2,maxRows: 5}" placeholder="Plase input invoiceNumber" />
             </FormItem>
             <FormItem label="customName">
                <customer-select style="z-index:9999"></customer-select>
                 <!-- <Input :disabled="hasSubmit" v-model="customerName" type="textarea" :autosize="{minRows: 2,maxRows: 5}" placeholder="Plase input customerName" /> -->
             </FormItem>
             <FormItem label="Code:">
                 <Input :disabled="hasSubmit" v-model="code" type="textarea" :autosize="{minRows: 2,maxRows: 5}" placeholder="Plase input code" />
             </FormItem>
             <FormItem label="Quantity:">
                 <Input :disabled="hasSubmit" v-model="quantity" type="textarea" :autosize="{minRows: 2,maxRows: 5}" placeholder="Plase input quantity" />
             </FormItem>
              <!-- <FormItem label=""> -->
                 <Button :disabled="hasSubmit" @click="handleSubmit" style="width:100px; display: inline-block;" type="primary">Add</Button>
             <!-- </FormItem> -->
              <!-- <FormItem label="" style="display: inline-block;" > -->
                 <Button :disabled="hasSubmit" @click="CompleteSome" style="width:100px;display:inline" type="primary">Deduct</Button>
             <!-- </FormItem> -->
         </Form>
       </Col>
       </Row>

  </div>
</template>
<script>
import API from '../../api/config'
import VueBootstrapTable from '../components/VueBootstrapTable.vue'
import customerSelect from  '../components/CustomerSelect/CustomerSelect.vue'
import $ from "jquery"

// :default-order-column="invoiceNumber"
    export default {
        components: {
            VueBootstrapTable,
            customerSelect
        },
        data () {
            return {
                    columnToSortBy: "invoiceNumber",
                    data:[],
                    current:{},
                    invoiceNumber:"",
                    customerName:"",
                    code:"",
                    quantity:0,
                    hasSubmit:false,
                    aj: {
                       enabled: true,
                       url: API.host+"/api/backorders?echo=2",
                       method: "GET",
                       delegate: false
                     },
                    columns:[{
                              title: 'id',
                              visible: true,
                          },  {
                              title: 'customName',
                              visible: true,
                              editable: true,
                          },  {
                              title: 'invoiceNumber',
                              visible: true,
                              editable: true,
                          }
                        ],
                    dialog:{
                          show:false,
                          title:{
                                    			text:'Back Order Detail',
                                  			  className:'xa-bg-blue'	//标题样式类名，包含`background`、`color`即可
                          	},
                        buttons:[
                                  			/**
                                  				`@text`：按钮文字
                                  				`@className`：按钮样式类名，包含`background`、`color`即可（内置一部分）
                                  				`@clickEvent`：按钮点击事件后监听的事件名
                                  			*/
                                  			{text:'Cancel',className:'xa-bg-red-click',clickEvent:'cancel'},
                                  			{text:'Complete',className:'xa-bg-blue-click',clickEvent:'confirm'}
                         	]
        	            }
          }
        },
        methods: {
          // getData(){
          // this.loading = true;
          //  this.$http.get(API.host+"/api/backorders").then(function(res){
          //                if(res.data){
          //                  this.backorders = res.data;
          //                }
          //            },function(res){
          //           });
          // },
          complete(id)
          {
            this.$http.get(API.host+"/api/backorders/complete/"+id).then(function(res){
                          this.getData ();
                      },function(res){
                     });
          },
          rowClicked($event, entry){
              // this.current = entry;
              let that = this;
              let content = "";

              for(var o in entry.orders)
　            {
                content +=  o +" x "+entry.orders[o] +"<br/>";
              }
              this.$modal.show('dialog', {
                  title:  entry.invoiceNumber,
                  text:content,
                  buttons: [
                    {
                      title: 'Complete',
                      handler: () => {
                          that.complete(entry.id);
                      }
                    },
                    {
                      title: 'Close'
                    }
                 ]
               });
          },
          onCellDataModifiedEvent(originalValue, newValue, columnTitle, entry){
            this.$http.post(API.host+"/api/backorders/update",entry, {  headers: {  'Content-Type': 'application/json'  }  }
                ).then(function(res){
                            // this.data = {};
                             console.log("success！");
                              // console.log(res.data.body.message);
                  },function(res){
                                 // this.data = {};
                                       console.log(res.body.message);
                  });
          },
          onAjaxLoadedEvent( data ) {
              console.log("ajaxLoadedEvent - data : " + data );
          },
          onAjaxLoadingError( data ) {
              console.log("ajaxLoadedEvent - data : " + data );
          },
          handleSubmit(){
            // let cn = $(".v-autocomplete-input").val();
            let url = API.host+"/api/backorders/addItem?invoiceNumber="+this.invoiceNumber+"&customerName="+$(".v-autocomplete-input").val()+"&code="+this.code+"&quantity="+this.quantity
            this.$http.get(url).then(function(res){
                                  if(res.data)
                                  {
                                    alert(res.data);
                                  }
                              },function(res){
                                  alert(res.status)
                              });
          },
          CompleteSome(){
            // let cn = $(".v-autocomplete-input").val();
            let url = API.host+"/api/backorders/removeItem?invoiceNumber="+this.invoiceNumber+"&customerName="+$(".v-autocomplete-input").val()+"&code="+this.code+"&quantity="+this.quantity
            this.$http.get(url).then(function(res){
                                  if(res.data)
                                  {
                                    alert(res.data);
                                  }
                              },function(res){
                                  alert(res.status)
                              });
          },
      }// end of methods
    }
</script>
