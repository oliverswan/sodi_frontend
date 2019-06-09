<template>
  <div>
    <Row style="margin-top:10px">
       <Col span="24">
         <orders-table :columns="columns"  :invoice="order" :isCreate="isCreate"></orders-table>
         <!-- <Table  :columns="columns"  :data="invoice.items" border></Table> :items="invoice.items" -->
       </Col>
       </Row>
 <Row style="margin-top:10px">
       <Col span="24">
        <i-button type="primary" style="margin:10px"  @click.prevent="addCart">Add to itaka cart</i-button>
       </Col>
       </Row>

       
  </div>
</template>
<script>
import OrdersTable from '../components/ordersTable/ordersTable.vue';
import API from '../../api/config';
import $ from "jquery";

  export default {
       components:{
         OrdersTable
       },
        data () {
            return {
               order:{},
               isCreate:false,
               columns:["code","quantity"]

              // loading:true,
              // columns:[{
              //     title: 'Seq',
              //     type: 'index',
              //     width: 80,
              //     align: 'center'
              // },
              // {
              //     title: 'Code',
              //     align: 'center',
              //     key: 'inventoryItemCode'
              // },
              // {
              //     title: 'Description',
              //     align: 'center',
              //     key: 'description',
              //     editable: false
              // },
              // {
              //     title: 'Quantity',
              //     align: 'center',
              //     key: 'quantity',
              //     editable: false
              // },
              // {
              //     title: 'UnitAmount',
              //     align: 'center',
              //     key: 'unitAmount',
              //     editable: false
              // },
              // {
              //     title: 'AccountCode',
              //     align: 'center',
              //     key: 'accountCode',
              //     editable: false
              // },{
              //     title: 'TaxType',
              //     align: 'center',
              //     key: 'taxType',
              //     editable: false
              // },{
              //     title: 'Total',
              //     align: 'center',
              //     key: 'totalamount',
              //     editable: false
              // },  {
              //     title: 'Operation',
              //     align: 'center',
              //     width: 120,
              //     key: 'handle',
              //     handle: ['delete']
              // }]
            }
        },
        created (){
          if(this.$route.params)
          {
            this.order = this.$route.params;
            if(this.order.isCreate)
            {
              this.isCreate = true;
            }
          }
        },
        methods: {
          getData () {
          },
          addCart(){
            let url = API.host+"/api/orders/addcart?id=" + this.order.id;
                this.$http.get(url).then(function(res){
                        if(res.data){
                            alert(res.data);
                        }
                    },function(res){
                        alert(res.data)
                  });
          }
         
      }// end of methods
    }
</script>
