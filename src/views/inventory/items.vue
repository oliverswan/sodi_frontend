<template>
  <div>
       <Row>
       <Col span="24">
         <items-table
           :columns="columns"
           :values="data"
           :show-filter="true"
           :sortable="true"
           :ajax="aj"
           :paginated="true"
           :filter-case-sensitive="false"
           :default-order-direction="true"
           :options="options"
           @cellDataModifiedEvent="onCellDataModifiedEvent"
           @ajaxLoadedEvent="onAjaxLoadedEvent"
           @ajaxLoadingErro="onAjaxLoadingError"
           >
         </items-table>
       </Col>
       </Row>
       <Row>
       <Col span="24">
         <Form ref="item" :label-width="100">
             <FormItem label="Code" prop="opinion">
                 <Input :disabled="hasSubmit" v-model="current.code" type="textarea" :autosize="{minRows: 2,maxRows: 5}" placeholder="Plase input contactName name" />
             </FormItem>
             <FormItem label="Name：">
                 <Input :disabled="hasSubmit" v-model="current.name" type="textarea" :autosize="{minRows: 2,maxRows: 5}" placeholder="Plase input personName" />
             </FormItem>
             <FormItem label="Price：">
                 <Input :disabled="hasSubmit" v-model="current.price" type="textarea" :autosize="{minRows: 2,maxRows: 5}" placeholder="Plase input emailAddress" />
             </FormItem>
             <FormItem label="cprice：">
                 <Input :disabled="hasSubmit" v-model="current.cprice" type="textarea" :autosize="{minRows: 2,maxRows: 5}" placeholder="Plase input poAddressLine1" />
             </FormItem>
             <FormItem label="GL：">
                 <Input :disabled="hasSubmit" v-model="current.accountCode" type="textarea" :autosize="{minRows: 2,maxRows: 5}" placeholder="Plase input GL" />
             </FormItem>
             <FormItem label="">
                 <Button :disabled="hasSubmit" @click="handleSubmit" style="width:100px;" type="primary">Add</Button>
             </FormItem>
         </Form>
       </Col>
       </Row>
  </div>
</template>
<script>
// :default-order-column="code"
import API from '../../api/config';
import ItemsTable from '../components/ItemsTable/ItemsTable.vue';

    export default {
        components: {
            ItemsTable
        },
        data () {
            return {
              data:[],
              current:{},
              hasSubmit:false,
              options: {
                undefinedText: 'n/a',
                showPaginationSwitch: true,
              },
              aj: {
                 enabled: true,
                 url: API.host+"/api/items?echo=1",
                 method: "GET",
                 delegate: false
               },
              columns:[{
                  title: 'id',
                  visible: true,
              },
              {
                  title: 'code',
                  visible: true,
                  editable: true,
              },
              {
                title: 'name',
                visible: true,
                editable: true,
              },
              {
                title: 'price',
                visible: true,
                editable: true,
              },
              {
                title: 'stock',
                visible: true,
                editable: true,
              },
              {
                title: 'soldThisYear',
                visible: true,
                editable: true,
              },
              {
                title: 'spm',
                visible: true,
                editable: true,
              },
              {
                title: 'msoh',
                visible: true,
                editable: true,
              },
              {
                title: 'weight',
                visible: true,
                editable: true,
              },
              {
                title: 'accountCode',
                visible: true,
                editable: true,
              },
              {
                title: 'coming',
                visible: true,
                editable: true,
              }, {
                  title: "Action",
                  name:"code",
                  visible: true,
                  renderfunction: this.renderTableAction
              }
              ]
            }
        },
        methods:{
              renderTableAction(colname, entry){
                     // glyphicon-ok glyphicon-remove
                   return  Vue.compile('<div class="btn-group" role="group"  ref="actiondiv" >'+
                        '  <button type="button" class="btn btn-sm btn-primary"><span class="glyphicon " @click="receiveClick" aria-hidden="true">Receive</span></button>'+
                        '  <button type="button" class="btn btn-sm btn-danger"><span class="glyphicon " aria-hidden="true">'+entry.coming+'</span></button>'+
                        '</div>');

                    //  return '<div class="btn-group" role="group"  ref="actiondiv" >'+
                    //     '  <button type="button" class="btn btn-sm btn-primary"><span class="glyphicon " @click.stop="receiveClick" aria-hidden="true">Receive</span></button>'+
                    //     '  <button type="button" class="btn btn-sm btn-danger"><span class="glyphicon " aria-hidden="true">'+entry.coming+'</span></button>'+
                    //     '</div>';
              },
              receiveClick(){
                  alert("xxx");
              },
              onCellDataModifiedEvent(originalValue, newValue, columnTitle, entry){
                this.$http.post(API.host+"/api/items/save",entry, {  headers: {  'Content-Type': 'application/json'  }  }
                    ).then(function(res){
                                // this.data = {};
                                 console.log("success！");
                                  // console.log(res.data.body.message);
                      },function(res){
                                     // this.data = {};
                                           // console.log(res.body.message);
                      });
              },
              onAjaxLoadedEvent( data ) {
                  console.log("ajaxLoadedEvent - data : " + data );
              },
              onAjaxLoadingError( data ) {
                  console.log("ajaxLoadedEvent - data : " + data );
              },
              handleSubmit(){
                this.$http.post(API.host+"/api/items/add",this.current, {  headers: {  'Content-Type': 'application/json'  }  }
                    ).then(function(res){
                                // this.data = {};
                                 console.log("success！");
                                  // console.log(res.data.body.message);
                      },function(res){
                                     // this.data = {};
                                           console.log(res.body.message);
                      });
              }
        }
    }
</script>
