<template>
  <div>
    <Row>
      <Col>
          <Card shadow>
                <p slot="title">Inovice Information: </p>
                <p><b> Invoice Number:   </b>{{invoice.invoiceNumber}}&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<b> Contact Name:   </b>{{invoice.contactName}}</p>
                <p><b> Invoice Date:   </b>{{invoice.invoiceDate}}&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<b> Due Date:   </b>{{invoice.dueDate}}</p>
                <p><b> Total Amount:   </b>{{invoice.subtotals}}&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</p>
            </Card>

            <b> Customer Note:   </b>
            <Input v-model="invoice.customerNote" type="textarea" :autosize="{minRows: 2,maxRows: 5}" placeholder="Plase input customer note" />
            <b> Order Note:   </b>
            <Input v-model="invoice.orderNote" type="textarea" :autosize="{minRows: 2,maxRows: 5}" placeholder="Plase input order note" />
      </Col>
    </Row>

    <Row style="margin-top:10px">
       <Col span="24">
         <edit-table :columns="columns"  :invoice="invoice" ></edit-table>
         <!-- <Table  :columns="columns"  :data="invoice.items" border></Table> :items="invoice.items" -->
       </Col>
       </Row>
  </div>
</template>
<script>
import editTable from '../components/editTable/editTable.vue';

  export default {
       components:{
         editTable
       },
        data () {
            return {
               invoice:{},
               columns:["inventoryItemCode","description","quantity","unitAmount","totalamounts","gsts","subtotals","accountCode"]

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
            this.invoice = this.$route.params;
          }
        },
        methods: {
          getData () {
          }
      }// end of methods
    }
</script>
