<template>
  <div>
       <Row>
       <Col span="24">
         <tracking-table
           :columns="columns"
           :values="data"
           :show-filter="true"
           :sortable="true"
           :paginated="true"
           :ajax="aj"
           
           :filter-case-sensitive="false"
           :default-order-direction="true"
           @cellDataModifiedEvent="onCellDataModifiedEvent"
           @ajaxLoadedEvent="onAjaxLoadedEvent"
           @ajaxLoadingErro="onAjaxLoadingError"
           >

           <!-- :rowClickHandler="rowClicked" -->
         </tracking-table>
         <!-- <v-dialog/>
         <modal name="detail">
          {{this.current.invoiceNumber}}
         </modal> -->
       </Col>
       </Row>
       <Row>
       <Card shadow>

    <!-- private String customerName;
    private String partsType;
    private String date;
    private String freightType;
    private List<String> proFormaFileUrls;
    private String proFormaNumber;
    private int proformaInXero;//0,1
    private int depositPaymentStatus;//0,1
    private List<String> depositPaymentUrls;
    private String productionDate;
    private int sodiInvoiceInXero;//0,1
    private int proformaCancelled;//0,1
    private int balancePaymentStatus;//0,1
    private List<String> balancePaymentUrls;
    private String dispatchDate;
    private List<String> shippingPreAlertUrls;
    private String deliveryDate; -->
                 Case Id : <Input v-model="currentId" type="text" style="width:5%" /><br>
                 <b> Upload proforma:   </b><input type="file" value="file" id="file1" @change="upload_proforma"></Input>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
                 <b> Upload deposit payment:   </b><input type="file" value="" id="file2" @change="upload_deposit"></Input>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
                 <b> Upload balance payment:   </b><input type="file" value="" id="file3" @change="upload_balance"></Input>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
                 <b> Upload shipping pre alert:   </b><input type="file" value="" id="file4" @change="upload_shipingalert"></Input>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
               
            </Card>
            </Row>
            <Row>
         <i-button type="primary" style="margin:10px"  @click.prevent="create">Create New Tracking</i-button>
       </Row>
       <!-- <Row>
       <Col span="24" style="margin-top:50px">
         <Form ref="item" :label-width="100">
             
             <FormItem label="invoiceNumber" prop="opinion">
                 <Input :disabled="hasSubmit" v-model="invoiceNumber" type="textarea" :autosize="{minRows: 2,maxRows: 5}" placeholder="Plase input invoiceNumber" />
             </FormItem>
             <FormItem label="customName">
                <customer-select style="z-index:9999"></customer-select>
             </FormItem>
             <FormItem label="Code:">
                 <Input :disabled="hasSubmit" v-model="code" type="textarea" :autosize="{minRows: 2,maxRows: 5}" placeholder="Plase input code" />
             </FormItem>
             <FormItem label="Quantity:">
                 <Input :disabled="hasSubmit" v-model="quantity" type="textarea" :autosize="{minRows: 2,maxRows: 5}" placeholder="Plase input quantity" />
             </FormItem>
                 <Button :disabled="hasSubmit" @click="handleSubmit" style="width:100px; display: inline-block;" type="primary">Add</Button>
                 <Button :disabled="hasSubmit" @click="CompleteSome" style="width:100px;display:inline" type="primary">Deduct</Button>
                 <Button :disabled="hasSubmit" @click="markOrdered" style="width:100px;display:inline" type="primary">Mark ordered</Button>
         </Form>
       </Col>
       </Row> -->

  </div>
</template>
<script>
import API from '../../api/config';
import Vue from 'vue';
import TrackingTable from '../components/trackingTable/trackingTable.vue';
// import VueBootstrapTable from '../components/VueBootstrapTable.vue';
// import customerSelect from  '../components/CustomerSelect/CustomerSelect.vue'
import $ from "jquery";
import axios from 'axios';

// :default-order-column="invoiceNumber"
    export default {
        components: {
            TrackingTable
            // ,
            // customerSelect
        },
        data () {
            return {
                    columnToSortBy: "id",
                    data:[],
                    currentId:0,
                    trackings:[],
                    current:{},
                    hasSubmit:false,
                    aj: {
                       enabled: true,
                       url: API.host+"/potracking?echo=5",
                       method: "GET",
                       delegate: false
                     },
                    columns:[{
                              title: 'id',
                              visible: true,
                          },  {
                              title: 'date',
                              visible: true,
                              editable: true,
                          },{
                              title: 'customerName',
                              visible: true,
                              editable: true,
                          } , {
                              title: 'partsType',
                              visible: true,
                              editable: true,
                          },  {
                              title: 'freightType',
                              visible: true,
                              editable: true,
                          }
                          // ,  {
                          //     title: 'proFormaFileUrls',
                          //     visible: true,
                          //     editable: true,
                          // }
                          ,  {
                              title: 'proFormaNumber',
                              visible: true,
                              editable: true,
                          }
                          ,  {
                              title: 'proformaInXero',
                              visible: true,
                              editable: true,
                          },  {
                              title: 'depositPaymentStatus',
                              visible: true,
                              editable: true,
                          }, 
                          //  {
                          //     title: 'depositPaymentUrls',
                          //     visible: true,
                          //     editable: true,
                          // }, 
                           {
                              title: 'productionDate',
                              visible: true,
                              editable: true,
                          },  {
                              title: 'sodiInvoiceInXero',
                              visible: true,
                              editable: true,
                          },  {
                              title: 'proformaCancelled',
                              visible: true,
                              editable: true,
                          },  {
                              title: 'balancePaymentStatus',
                              visible: true,
                              editable: true,
                          },  
                          // {
                          //     title: 'balancePaymentUrls',
                          //     visible: true,
                          //     editable: true,
                          // }, 
                          // {
                          //     title: 'shippingPreAlertUrls',
                          //     visible: true,
                          //     editable: true,
                          // }, 
                           {
                              title: 'deliveryDate',
                              visible: true,
                              editable: true,
                          },
                           {
                              title: "Action",
                              name:"code",
                              visible: true,
                              renderfunction: this.renderTableAction
                          }
                        ],
                    // dialog:{
                    //       show:false,
                    //       title:{
                    //                 			text:'Back Order Detail',
                    //               			  className:'xa-bg-blue'	//标题样式类名，包含`background`、`color`即可
                    //       	},
                    //     buttons:[
                    //               			/**
                    //               				`@text`：按钮文字
                    //               				`@className`：按钮样式类名，包含`background`、`color`即可（内置一部分）
                    //               				`@clickEvent`：按钮点击事件后监听的事件名
                    //               			*/
                    //               			{text:'Cancel',className:'xa-bg-red-click',clickEvent:'cancel'},
                    //               			{text:'Complete',className:'xa-bg-blue-click',clickEvent:'confirm'}
                    //      	]
        	          //   }
          }
        },
        methods: {
          getData(){
          this.loading = true;
           this.$http.get(API.host+"/api/potracking").then(function(res){
                         if(res.data){
                           this.trackings = res.data;
                         }
                     },function(res){
                    });
          },
          downloadFilesClick(){

          },
          renderTableAction(colname, entry){
                     // glyphicon-ok glyphicon-remove
                   return  Vue.compile(
                     
                     '<div class="btn-group" role="group"  ref="actiondiv" ><button type="button" class="btn btn-sm btn-primary"><span class="glyphicon " @click="downloadFilesClick" aria-hidden="true">View Files</span></button></div>'
                        // +
                        // '  <button type="button" class="btn btn-sm btn-danger"><span class="glyphicon " aria-hidden="true">'+entry.coming+'</span></button>'+
                        // '</div>'
                        );

                    //  return '<div class="btn-group" role="group"  ref="actiondiv" >'+
                    //     '  <button type="button" class="btn btn-sm btn-primary"><span class="glyphicon " @click.stop="receiveClick" aria-hidden="true">Receive</span></button>'+
                    //     '  <button type="button" class="btn btn-sm btn-danger"><span class="glyphicon " aria-hidden="true">'+entry.coming+'</span></button>'+
                    //     '</div>';
              },
          // complete(id)
          // {
          //   this.$http.get(API.host+"/api/backorders/complete/"+id).then(function(res){
                        
          //                 // this.getData ();
          //             },function(res){
          //            });
          // },
          rowClicked($event, entry){
              this.current = entry;
              alert("You choose : " + this.current.id +" "+this.current.customerName);
              // console.log(this.current);
//               let that = this;
//               let content = "";

//               for(var o in entry.orders)
// 　            {
//                 content +=  o +" x "+entry.orders[o] +"<br/>";
//               }
//               this.$modal.show('dialog', {
//                   title:  entry.invoiceNumber,
//                   text:content,
//                   buttons: [
//                     {
//                       title: 'Complete',
//                       handler: () => {
//                           that.complete(entry.id);
//                       }
//                     },
//                     {
//                       title: 'Close'
//                     }
//                  ]
//                });
          },
          onCellDataModifiedEvent(originalValue, newValue, columnTitle, entry){
            this.$http.post(API.host+"/api/potracking/update",entry, {  headers: {  'Content-Type': 'application/json'  }  }
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
            // let url = API.host+"/api/backorders/addItem?invoiceNumber="+this.invoiceNumber+"&customerName="+$(".v-autocomplete-input").val()+"&code="+this.code+"&quantity="+this.quantity
            // this.$http.get(url).then(function(res){
            //                       if(res.data)
            //                       {
            //                         alert(res.data);
            //                       }
            //                   },function(res){
            //                       alert(res.status)
            //                   });
          },upload_proforma(e){
           
              let formData = new FormData();
              formData.append('file', e.target.files[0]);
              let url = API.host+'/potracking/upload/proforma/'+this.currentId;//
              alert(url);
              let config = {
                headers:{'Content-Type':'multipart/form-data'},
                responseType: 'blob'
              };
              
            axios.post(url,formData, config).then(function (response) {
                console.log(response.data)

                // const content = response.data;
                //               const blob = new Blob([content]);
                //               const fileName = `price.csv`;
                //               if ("download" in document.createElement("a")) {
                //                   // 非IE下载
                //                   const elink = document.createElement("a");
                //                   elink.download = fileName;
                //                   elink.style.display = "none";
                //                   elink.href = URL.createObjectURL(blob);
                //                   document.body.appendChild(elink);
                //                   elink.click();
                //                   URL.revokeObjectURL(elink.href); // 释放URL 对象
                //                   document.body.removeChild(elink);
                //               } else {
                //                   // IE10+下载
                //                   navigator.msSaveBlob(blob, fileName);
                //               }
              });
          },upload_deposit(e){
              let formData = new FormData();
              formData.append('file', e.target.files[0]);
              let url = API.host+'/potracking/upload/depposit/'+this.currentId;
              let config = {
                headers:{'Content-Type':'multipart/form-data'},
                responseType: 'blob'
              };
              
            axios.post(url,formData, config).then(function (response) {
                console.log(response.data)

                // const content = response.data;
                //               const blob = new Blob([content]);
                //               const fileName = `price.csv`;
                //               if ("download" in document.createElement("a")) {
                //                   // 非IE下载
                //                   const elink = document.createElement("a");
                //                   elink.download = fileName;
                //                   elink.style.display = "none";
                //                   elink.href = URL.createObjectURL(blob);
                //                   document.body.appendChild(elink);
                //                   elink.click();
                //                   URL.revokeObjectURL(elink.href); // 释放URL 对象
                //                   document.body.removeChild(elink);
                //               } else {
                //                   // IE10+下载
                //                   navigator.msSaveBlob(blob, fileName);
                //               }
              });
          },upload_balance(e){
              let formData = new FormData();
              formData.append('file', e.target.files[0]);
              let url = API.host+'/potracking/upload/balance/'+this.currentId;
              let config = {
                headers:{'Content-Type':'multipart/form-data'},
                responseType: 'blob'
              };
              
            axios.post(url,formData, config).then(function (response) {
                console.log(response.data)

                // const content = response.data;
                //               const blob = new Blob([content]);
                //               const fileName = `price.csv`;
                //               if ("download" in document.createElement("a")) {
                //                   // 非IE下载
                //                   const elink = document.createElement("a");
                //                   elink.download = fileName;
                //                   elink.style.display = "none";
                //                   elink.href = URL.createObjectURL(blob);
                //                   document.body.appendChild(elink);
                //                   elink.click();
                //                   URL.revokeObjectURL(elink.href); // 释放URL 对象
                //                   document.body.removeChild(elink);
                //               } else {
                //                   // IE10+下载
                //                   navigator.msSaveBlob(blob, fileName);
                //               }
              });
          },upload_shipingalert(e){
              let formData = new FormData();
              formData.append('file', e.target.files[0]);
              let url = API.host+'/potracking/upload/shippingalert/'+this.currentId;
              let config = {
                headers:{'Content-Type':'multipart/form-data'},
                responseType: 'blob'
              };
              
            axios.post(url,formData, config).then(function (response) {
                console.log(response.data)

                // const content = response.data;
                //               const blob = new Blob([content]);
                //               const fileName = `price.csv`;
                //               if ("download" in document.createElement("a")) {
                //                   // 非IE下载
                //                   const elink = document.createElement("a");
                //                   elink.download = fileName;
                //                   elink.style.display = "none";
                //                   elink.href = URL.createObjectURL(blob);
                //                   document.body.appendChild(elink);
                //                   elink.click();
                //                   URL.revokeObjectURL(elink.href); // 释放URL 对象
                //                   document.body.removeChild(elink);
                //               } else {
                //                   // IE10+下载
                //                   navigator.msSaveBlob(blob, fileName);
                //               }
              });
          },create(){
              // let newinvoice ={};
              // newinvoice.items = [];
              // newinvoice.isCreate = true;
              this.$router.push({
                  name: 'editTracking'
                  // ,
                  // params:newinvoice
              });
            },
          // ,
          // CompleteSome(){
          //   // let cn = $(".v-autocomplete-input").val();
          //   let url = API.host+"/api/backorders/removeItem?invoiceNumber="+this.invoiceNumber+"&customerName="+$(".v-autocomplete-input").val()+"&code="+this.code+"&quantity="+this.quantity
          //   this.$http.get(url).then(function(res){
          //                         if(res.data)
          //                         {
          //                           alert(res.data);
          //                         }
          //                     },function(res){
          //                         alert(res.status)
          //                     });
          // },
          //  markOrdered(){
          //   // let cn = $(".v-autocomplete-input").val();
          //   let url = API.host+"/api/backorders/markOrdered"
          //   this.$http.get(url).then(function(res){
          //                         if(res.data)
          //                         {
          //                           alert(res.data);
          //                         }
          //                     },function(res){
          //                         alert(res.status)
          //                     });
          // }
      }// end of methods
    }
</script>
