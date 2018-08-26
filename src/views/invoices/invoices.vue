<template>
  <div>
       <Row>
       <Col span="24">
               <can-edit-table
                   refs="draftTable"
                   v-model="data"
                   @on-cell-change="handleCellChange"
                   @on-change="handleChange"
                   @on-row-click="rowClick"
                   :editIncell="true"
                   :rowClassfunction="getRowClassName"
                   :columns-list="columns"
                   :loading="loading"
                   :hoverShow=true
               ></can-edit-table>
       </Col>
       </Row>
  </div>
</template>

<script>
import canEditTable from '../tables/components/canEditTable.vue';
import API from '../../api/config';
import $ from "jquery";
    export default {
      components: {
          canEditTable
      },
        data () {
            return {
              data:[],
              mapping:"",
              loading:false,
              columns:[{
                  title: 'Seq',
                  type: 'index',
                  width: 80,
                  align: 'center'
              },{
                  title: 'orderId',
                  align: 'center',
                  key: 'orderNumber',
                  editable: false
              },{
                  title: 'Reference',
                  align: 'center',
                  key: 'Reference',
                  editable: false
              },{
                  title: 'InvoiceNumber',
                  align: 'center',
                  key: 'invoiceNumber',
                  editable: false
              },
              {
                  title: 'Name',
                  align: 'center',
                  key: 'contactName'
              },
              {
                  title: 'Total',
                  align: 'center',
                  key: 'totalamount'
              },
              {
                       title: 'Action',
                       key: 'action',
                       width: 300,
                       align: 'center',
                       render: (h, params) => {
                         return  h('div',[ h('div',{style:{float:'left',margin:'auto',cursor:'pointer'},on: {
                                       click: () => {
                                         this.$http.get(API.host+"/api/mail/"+params.row.id).then(function(res){
                                                       console.log("success!!");
                                                       $("tr:contains('"+params.row.invoiceNumber+"')").addClass('green-row');
                                                   },function(res){
                                                        //alert(res.status)
                                                  });
                                       }
                                   }},[h('Icon',{ props:{type: 'ios-email-outline'},style:{ fontSize: '38px',
                                     color: '#191970',
                                     width: '20px',
                                     margin:'10px',
                                     height:'20px'}},'内容2')],

                         '内容1'),h('div',{style:{float:'left',cursor:'pointer'},on: {
                                       click: () => {
                                           let url = API.host+"/api/reports/invoice/"+params.row.id;
                                           window.open(url);
                                       }
                                   }},[h('Icon',{ props:{type: 'document'},style:{ fontSize: '38px',
                                     color: '#FF4500',
                                     width: '20px',
                                     margin:'10px',
                                     height:'30px'}},'内容2')],

                         '内容1'),h('div',{style:{float:'left',cursor:'pointer'},on: {
                                       click: () => {
                                        this.view(params.index);
                                       }
                                   }},[h('Icon',{ props:{type: 'document'},style:{ fontSize: '38px',
                                     color: '#191970',
                                     width: '20px',
                                     margin:'10px',
                                     height:'30px'}},'内容2')],

                         '内容1')],'');




                       }// end of render
              }//end of ele
              ]// end of columns
            }//endof return data
        },// end of data

        mounted () {
            this.getData();
        },
        methods: {
          getData () {
            // this.data =[{"name":"field","values":"\"aaaa\",\"bbbb\",\"cccc\",\"dddddddd\""}];
            this.loading = true;
           //  let _this = this;
           this.$http.get(API.host+"/api/invoices").then(function(res){
                        this.loading = false;
                         if(res.data){
                           this.data = res.data;
                         }
                     },function(res){
                       this.loading = false;
                          //alert(res.status)
                    });
          },
          getRowClassName (row, index) {

                      if(row.status == 3)
                      {
                            return 'green-row';
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
            approveAll() {
              this.$http.post(API.host+"/api/invoices/approveall2",this.mapping, {  headers: {  'Content-Type': 'text/plain'  }  }
                  ).then(function(res){
                              // this.data = {};
                               console.log("success！");
                                // console.log(res.data.body.message);
                    },function(res){
                                   // this.data = {};
                                         console.log(res.body.message);
                    });


            }
      }// end of methods
    }
</script>
<style>
</style>
