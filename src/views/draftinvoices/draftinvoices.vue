<template>

  <div>
      <Table border ref="invoTable"  :row-class-name="getRowClassName" :columns="columns" :data="data" @on-select="tableSelected"></Table>
      <!-- <can-edit-table
                   :ref="table1"
                   v-model="data"
                   @on-cell-change="handleCellChange"
                   @on-change="handleChange"
                   @on-row-click="rowClick"
                   :editIncell="true"
                   :columns-list="columns"
                   :loading="loading"
                   :handlePage="handlePage"
                   :handlePageSize="handlePageSize"
                   :hoverShow=true
               ></can-edit-table> -->
               
       <!-- <Row>
       <Col span="24">
               <can-edit-table
                   v-model="data"
                   @on-cell-change="handleCellChange"
                   @on-change="handleChange"
                   @on-row-click="rowClick"
                   :editIncell="true"
                   :columns-list="columns"
                   :loading="loading"
                   :handlePage="handlePage"
                   :handlePageSize="handlePageSize"
                   :hoverShow=true
               ></can-edit-table>
       </Col>
      </Row> -->
       <v-dialog/>
         <modal name="detail">
         "ffffffffffffffff"
         </modal>
         <modal name="Processing"  :adaptive="true" width="100%" height="100%">
            <div>
                <h1>Processing!</h1>
            </div>     
         </modal>
      <Row>
        <Button @click="handleSelectAll(true)">Select All</Button>
        <Button @click="handleSelectAll(false)">Cancel All selected</Button>
        <i-button type="primary" style="margin:10px"  @click.prevent="approveAll">Approve Selected</i-button>
        <i-button type="primary" style="margin:10px"  @click.prevent="create">Create New Order</i-button>
        <i-button type="primary" style="margin:10px"  @click.prevent="refresh">Refresh</i-button>
      </Row>
  </div>
</template>
<script>
import canEditTable from '../tables/components/canEditTable.vue';
import API from '../../api/config';
import $ from "jquery"
// <Row>
//   <Col span="24">
//      <Input v-model="mapping" type="textarea"  :rows="20" placeholder="Enter mapping..." />
//  </Col>
// </Row>

    export default {
      components: {
          canEditTable
      },
        data () {
            return {
              data:[],
              mapping:"",
              loading:false,
              selected:[],
              columns:[
                 {
                        type: 'selection',
                        width: 60,
                        align: 'center'
                },  
                {
                  title: 'Seq',
                  type: 'index',
                  width: 80,
                  align: 'center'
              },{
                  title: 'InvoiceNumber',
                  align: 'center',
                  key: 'invoiceNumber',
                  editable: false
              },
              {
                  title: 'reference',
                  align: 'center',
                  key: 'reference',
                  editable: false
              },
              {
                  title: 'Name',
                  align: 'center',
                  key: 'contactName'
              },{
                  title: 'Amount',
                  align: 'center',
                  key: 'subtotals',
                  editable: false
              },

              {
                       title: 'Action',
                       key: 'action',
                       width: 450,
                       align: 'center',
                       render: (h, params) => {
                           return h('div', [
                                h('Button', {
                                   props: {
                                       type: 'primary',
                                       size: 'small'
                                   },
                                   style: {
                                       marginRight: '5px'
                                   },
                                   on: {
                                       click: () => {
                                         this.checkInventory(params.index);
                                       }
                                   }
                               }, 'Check Inventory'),
                               h('Button', {
                                   props: {
                                       type: 'primary',
                                       size: 'small'
                                   },
                                   style: {
                                       marginRight: '5px'
                                   },
                                   on: {
                                       click: () => {
                                         this.view(params.index);
                                       }
                                   }
                               }, 'Edit')
                               ,
                              
                               h('Button', {
                                   props: {
                                       type: 'primary',
                                       size: 'small'
                                   },
                                   style: {
                                       marginRight: '5px'
                                   },
                                   on: {
                                       click: () => {
                                          this.approveSingle(params.index);
                                       }
                                   }
                               }, 'Approve'),
                               h('Button', {
                                   props: {
                                       type: 'primary',
                                       size: 'small'
                                   },
                                   style: {
                                       marginRight: '5px'
                                   },
                                   on: {
                                       click: () => {
                                          this.goBO(params.index);
                                       }
                                   }
                               }, 'Add BO'),
                               h('Button', {
                                   props: {
                                       type: 'primary',
                                       size: 'small'
                                   },
                                   on: {
                                       click: () => {
                                          this.approvceBackorder(params.index);
                                       }
                                   }
                               }, 'Clear B/O'),
                               h('Button', {
                                   props: {
                                       type: 'primary',
                                       size: 'small'
                                   },
                                   on: {
                                       click: () => {
                                          this.stockBo(params.index);
                                       }
                                   }
                               }, 'Stock B/O')
                           ]);
                       }
                   }

            ]
            }
        },
        mounted () {
            this.getData();
        },
        methods: {
          getData () {
            // this.data =[{"name":"field","values":"\"aaaa\",\"bbbb\",\"cccc\",\"dddddddd\""}];
            this.loading = true;
           //  let _this = this;
           this.$http.get(API.host+"/api/invoices/draft").then(function(res){
                        this.loading = false;
                         if(res.data){
                           this.data = res.data;
                         }
                     },function(res){
                       this.loading = false;
                          //alert(res.status)
                    });
          },handlePage(v)
          {

          },handlePageSize(v){

          },
          getRowClassName (row, index) {

                    if(row.pocountry !="Australia")
                    {
                         return 'green-row';
                    }else if(row.contactName === "The Kart Center"||row.contactName === "The Kart Centre")
                    {
                            return 'yellow-row';
                     }
                      return '';
            },

            view(index) {
                let invoice = this.data[index];
                this.$router.push({
                    name: 'viewInvoice',
                    params:invoice
                });
              },

            handleDel (val, index) {
                this.$Message.success('删除了第' + (index + 1) + '行数据');
            },
            handleCellChange (val, index, key) {
                // this.$Message.success('修改了第 ' + (index) + ' 行列名为 ' + key + ' 的数据');
                alert(this.data[0].values);
                // this.$Message.success(this.data[index].values);
            },
            rowClick () {
                alert("dd");
            },
            handleChange (val, index) {
                this.$Message.success('修改了第' + (index + 1) + '行数据');
            },
            refresh(){
                this.getData();
            },
            approveAll() {
                //'text/plain'
              this.$http.post(API.host+"/api/invoices/approveMulti",this.selected, {  headers: {  'Content-Type': 'application/json'  }  }
                  ).then(function(res){
                              // this.data = {};
                               console.log("success！");
                                this.getData();
                                // console.log(res.data.body.message);
                    },function(res){
                                   // this.data = {};
                                         console.log(res.body.message);
                    });


            },
             create(){
              let newinvoice ={};
              newinvoice.items = [];
              newinvoice.isCreate = true;
              this.$router.push({
                  name: 'viewInvoice',
                  params:newinvoice
              });
            },
             approveSingle(p) {
                 this.$modal.show('Processing');
              this.$http.post(API.host+"/api/invoices/approveSingle",this.data[p], {  headers: {  'Content-Type': 'application/json'  }  }
                  ).then(function(res){
                              // this.data = {};
                                this.$modal.hide('Processing');
                                alert(res.data);
                               this.getData();
                                // console.log(res.data.body.message);
                    },function(res){
                                   // this.data = {};
                                         console.log(res.body.message);
                    });
            },

            approvceBackorder(p) {
              this.$http.post(API.host+"/api/invoices/approveBackorder",this.data[p], {  headers: {  'Content-Type': 'application/json'  }  }
                  ).then(function(res){
                              // this.data = {};
                               alert(res.data);
                               this.getData();
                                // console.log(res.data.body.message);
                    },function(res){
                                   // this.data = {};
                                         console.log(res.body.message);
                    });
            },
            
            stockBo(p) {
              this.$http.post(API.host+"/api/invoices/stockBackorder",this.data[p], {  headers: {  'Content-Type': 'application/json'  }  }
                  ).then(function(res){
                              // this.data = {};
                               alert(res.data);
                               this.getData();
                                // console.log(res.data.body.message);
                    },function(res){
                                   // this.data = {};
                                         console.log(res.body.message);
                    });
            },

            checkInventory(index)
            {
                     let url = API.host+"/api/invoices/check/"+this.data[index].id;
                     //let that = this;
                     
                     this.$http.get(url).then(function(res){
                         if(res.data){
                           let content =res.data;
                           this.$modal.show('dialog', {
                            title: 'Inventory',
                            text: content,
                            buttons: [
                                {
                                title: 'Close'
                                }
                            ]
                    });
                         }
                     },function(res){
                       this.loading = false;
                          //alert(res.status)
                    });
            },
            goBO(index)
            {
                let url = API.host+"/api/invoices/gobo/"+this.data[index].id;
                     //let that = this;
                     
                     this.$http.get(url).then(function(res){
                         if(res.data){
                           let content =res.data;
                           this.$modal.show('dialog', {
                            title: 'Go Bo',
                            text: content,
                            buttons: [
                                {
                                title: 'Close'
                                }
                            ]
                    });
                         }
                     },function(res){
                       this.loading = false;
                          //alert(res.status)
                    });
            },
              handleSelectAll (status) {
                //   console.log(this.$refs.table1);
                this.$refs.invoTable.selectAll(status);
            },
            tableSelected(selectedArray,currentSelected){
                    this.selected = selectedArray;
            }
      }// end of methods
    }
</script>
<style>
.green-row td{
   background-color: #8FBC8F;
}
.yellow-row td{
   background-color: rgb(223, 238, 20);
}
</style>
