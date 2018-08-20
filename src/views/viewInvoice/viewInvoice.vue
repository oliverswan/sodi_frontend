<template>
  <div>
    <Row>
      <Col>
          <Card shadow>
                <p slot="title">Inovice Information: </p>
                <p><b> Invoice Number:   </b>{{invoice.invoiceNumber}}&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<b> Contact Name:   </b>{{invoice.contactName}}</p>
                <p><b> Invoice Date:   </b>{{invoice.invoiceDate}}&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<b> Due Date:   </b>{{invoice.dueDate}}</p>
                <p><b> Total Amount:   </b>{{invoice.totalamount}}&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</p>
            </Card>
      </Col>
    </Row>

       <Row>
       <Col span="24">
         <!-- :loading="loading" -->
         <Table  :columns="columns"  :data="invoice.items" border></Table>
       </Col>
       </Row>
  </div>
</template>
<script>
    export default {
        data () {
            return {
              invoice:{},
              // loading:true,
              columns:[{
                  title: 'Seq',
                  type: 'index',
                  width: 80,
                  align: 'center'
              },
              {
                  title: 'Code',
                  align: 'center',
                  key: 'inventoryItemCode'
              },
              {
                  title: 'Description',
                  align: 'center',
                  key: 'description',
                  editable: false
              },
              {
                  title: 'Quantity',
                  align: 'center',
                  key: 'quantity',
                  editable: false
              },
              {
                  title: 'UnitAmount',
                  align: 'center',
                  key: 'unitAmount',
                  editable: false
              },
              {
                  title: 'AccountCode',
                  align: 'center',
                  key: 'accountCode',
                  editable: false
              },{
                  title: 'TaxType',
                  align: 'center',
                  key: 'taxType',
                  editable: false
              },{
                  title: 'Total',
                  align: 'center',
                  key: 'totalamount',
                  editable: false
              },  {
                  title: 'Operation',
                  align: 'center',
                  width: 120,
                  key: 'handle',
                  handle: ['delete']
              }]
            }
        },
        mounted (){
          if(this.$route.params)
          {
            this.invoice = this.$route.params;
              console.log(this.invoice.items);
          }
        },
        methods: {
          getData () {

          }
      }// end of methods
    }
</script>
