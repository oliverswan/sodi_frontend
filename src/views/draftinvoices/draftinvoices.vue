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
                   :columns-list="columns"
                   :loading="loading"
                   :hoverShow=true
               ></can-edit-table>
       </Col>
       </Row>

       <Row>
         <Col span="24">
            <Input v-model="mapping" type="textarea"  :rows="20" placeholder="Enter mapping..." />
        </Col>
      </Row>

      <Row>
        <i-button type="primary" style="margin:10px"  @click.prevent="approveAll">Approve All Drafts</i-button>
      </Row>
  </div>
</template>
<script>
import canEditTable from '../tables/components/canEditTable.vue';
import API from '../../api/config';

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
                       title: 'Action',
                       key: 'action',
                       width: 150,
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
                                         this.view(params.index);
                                       }
                                   }
                               }, 'View'),
                               h('Button', {
                                   props: {
                                       type: 'error',
                                       size: 'small'
                                   },
                                   on: {
                                       click: () => {
                                           alert(params.index);
                                       }
                                   }
                               }, 'Delete')
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
