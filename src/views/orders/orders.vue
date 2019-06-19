<template>
  <div>
    <Row>
      <!-- 
        <b>Customer Name:  </b>
        <customer-select style=" width:15%;display:inline-block;z-index:9999"></customer-select>
        <b>Invoice Number:  </b><Input v-model="invoiceNumber" style="width: 10%"></Input>
        <i-button type="primary" style="margin:10px;"  @click.prevent="search">Search</i-button>
       <Row>-->
          <!-- :rowClassfunction="getRowClassName" -->
       <Col span="24">
               <can-edit-table
                   refs="orderTable"
                   v-model="data"
                   @on-cell-change="handleCellChange"
                   @on-change="handleChange"
                   @on-row-click="rowClick"
                   :editIncell="true"
                   :columns-list="columns"
                   :loading="loading"
                   :pageTotal="pageTotal"
                   :pageNum="pageNum"
                   :handlePage="handlePage"
                   :handlePageSize="handlePageSize"
                   :hoverShow="true"
               ></can-edit-table>
       </Col>
       </Row>
       <Row>
         <b>Numer Of Month:  </b><Input v-model="numerOfMonth" style="width: 10%"></Input>
         <i-button type="primary" style="margin:10px"  @click.prevent="create">Generate Order On Sales</i-button>
         <i-button type="primary" style="margin:10px"  @click.prevent="create">Generate Order On Backorder</i-button>
         <i-button type="primary" style="margin:10px"  @click.prevent="updateItakaId">Update ItakaShop Id</i-button>
       </Row>
  </div>
</template>

<script>
import canEditTable from '../tables/components/canEditTable.vue';
import customerSelect from  '../components/CustomerSelect/CustomerSelect.vue'
import API from '../../api/config';
import $ from "jquery";
    export default {
      components: {
          canEditTable,
          customerSelect
      },
        data () {
            return {
              data:[],
              numerOfMonth:3,
              loading:false,
              pageTotal: 0,
              pageNum: 1,
              pageSize: 10,
              columns:[{
                  title: 'Seq',
                  type: 'index',
                  width: 80,
                  align: 'center'
              },{
                  title: 'orderId',
                  align: 'center',
                  key: 'id',
                  editable: false
              },{
                  title: 'name',
                  align: 'center',
                  key: 'name',
                  editable: false
              },
              {
                       title: 'Action',
                       key: 'action',
                       width: 400,
                       align: 'center',
                       render: (h, params) => {
                         return  h('div',[
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
                         }, 'View'),
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
                                  //  this.download(params.index);
                                 }
                             }
                         }, 'Download'),
                        //   h('Button', {
                        //      props: {
                        //          type: 'primary',
                        //          size: 'small'
                        //      },
                        //      style: {
                        //          marginRight: '5px'
                        //      },
                        //      on: {
                        //          click: () => {
                        //            this.undo(params.index);
                        //          }
                        //      }
                        //  }, 'Out Stock'),
                        //  h('div',{style:{float:'left',margin:'auto',cursor:'pointer'},on: {
                        //                click: () => {
                        //                  this.$http.get(API.host+"/api/mail/"+params.row.id).then(function(res){
                        //                                $("tr:contains('"+params.row.invoiceNumber+"')").addClass('green-row');
                        //                            },function(res){
                        //                                 //alert(res.status)
                        //                           });
                        //                }
                        //            }},[h('Icon',{ props:{type: 'ios-email-outline'},style:{ fontSize: '38px',
                        //              color: '#191970',
                        //              width: '20px',
                        //              margin:'10px',
                        //              height:'20px'}},'Send Invoice')],

                        //  '内容1'),h('div',{style:{float:'left',margin:'auto',cursor:'pointer'},on: {
                        //                click: () => {
                        //                   this.view(params.index);
                        //                }
                        //            }},[h('Icon',{ props:{type: 'ios-email-outline'},style:{ fontSize: '38px',
                        //              color: '#228B22',
                        //              width: '20px',
                        //              margin:'10px',
                        //              height:'20px'}},'Edit')],

                        //  '内容1'),h('div',{style:{float:'left',cursor:'pointer'},on: {
                        //                click: () => {
                        //                    let url = API.host+"/api/reports/invoice/"+params.row.id;
                        //                    window.open(url);
                        //                }
                        //            }},[h('Icon',{ props:{type: 'document'},style:{ fontSize: '38px',
                        //              color: '#FF4500',
                        //              width: '20px',
                        //              margin:'10px',
                        //              height:'30px'}},'Invoice')],

                        //  '内容1'),h('div',{style:{float:'left',cursor:'pointer'},on: {
                        //                click: () => {
                        //                 let url = API.host+"/api/reports/delivery/"+params.row.id;
                        //                 window.open(url);
                        //                }
                        //            }},[h('Icon',{ props:{type: 'document'},style:{ fontSize: '38px',
                        //              color: '#191970',
                        //              width: '20px',
                        //              margin:'10px',
                        //              height:'30px'}},'Delivery')],

                        //  '内容1')
                        ],'');
                       }// end of render
              }//end of ele
              ]// end of columns
            }//endof return data
        },// end of data

        mounted () {
            this.getData ();
        },
        methods: {
          handlePage(value) {
            this.pageNum = value
            console.log("Page "+value);
            this.getData ()
          },
          view(index) {
              let order = this.data[index];
              console.log("startA");
              console.log(order);
              this.$router.push({
                  name: 'viewSodiOrder',
                  params:order
              });
            },
          handlePageSize(value) {
            this.pageSize = value
            this.getData ()
          },
          getData () {
            // this.data =[{"name":"field","values":"\"aaaa\",\"bbbb\",\"cccc\",\"dddddddd\""}];
            this.loading = true;
           //  let _this = this;
           let url = API.host+"/api/orders";
            url = url + "?pageNum=" + this.pageNum + '&pageSize=' + this.pageSize;
           this.$http.get(url).then(function(res){
                        this.loading = false;
                         if(res.data){
                            //  console.log(res.data.data[0]);
                           this.data = res.data.data;
                           this.pageTotal = parseInt(res.data.message)
                         }
                     },function(res){
                       this.loading = false;
                          //alert(res.status)
                    });
          },
          // getRowClassName (row, index) {

          //             if(row.status == 3)
          //             {
          //                   return 'green-row';
          //             }
          //             return '';
          //   },

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
            },download(index) {
            },updateItakaId() {
                // let url = API.host+"/api/orders/create?month=" + this.numerOfMonth;
                // this.$http.get(url).then(function(res){
                //         if(res.data){
                //             this.getData();
                //         }
                //     },function(res){
                //         alert(res.data)
                //   });

                let url = API.host+"/api/items/updateitakaid";
                this.$http.get(url).then(function(res){
                        if(res.data){
                            this.getData();
                        }
                    },function(res){
                        alert(res.data)
                  });

            },create() {
                let url = API.host+"/api/orders/create?month=" + this.numerOfMonth;
                this.$http.get(url).then(function(res){
                        if(res.data){
                            this.getData();
                        }
                    },function(res){
                        alert(res.data)
                  });
            },create() {
                let url = API.host+"/api/orders/createbackorder";
                this.$http.get(url).then(function(res){
                        if(res.data){
                            this.getData();
                        }
                    },function(res){
                        alert(res.data)
                  });
            }
      }// end of methods
    }
</script>
<style>
</style>
