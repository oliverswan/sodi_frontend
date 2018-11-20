<template>
  <div>
    <Row>
      <Col>
          <Card shadow>
                <p slot="title">Inovice Information: </p>
                <p><b> Invoice Number:   </b><Input v-model="invoice.invoiceNumber" style="width: 20%"></Input>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<b> Contact Name:   </b><customer-select @changed="cusNameChanged" :initValue="contactName" style="width:20%;display:inline-block;z-index:9999" id='cusName'></customer-select></p>
                <p><b> Invoice Date:   </b>{{invoice.invoiceDate}}&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<b> Due Date:   </b>{{invoice.dueDate}}</p>
                <p><b> Total Amount:   </b>{{invoice.subtotals}}&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<b>Reference: </b><Input v-model="invoice.reference" style="width: 20%"/></p>
            </Card>

            <b> Customer Note:   </b>
            <Input v-model="invoice.customerNote" type="textarea" :autosize="{minRows: 2,maxRows: 5}" placeholder="Plase input customer note" />
            <b> Order Note:   </b>
            <Input v-model="invoice.orderNote" type="textarea" :autosize="{minRows: 2,maxRows: 5}" placeholder="Plase input order note" />
      </Col>
    </Row>

    <Row style="margin-top:10px">
       <Col span="24">
         <edit-table :columns="columns"  :invoice="invoice" :isCreate="isCreate"></edit-table>
         <!-- <Table  :columns="columns"  :data="invoice.items" border></Table> :items="invoice.items" -->
       </Col>
       </Row>
  </div>
</template>
<script>
import editTable from '../components/editTable/editTable.vue';
import customerSelect from  '../components/CustomerSelect/CustomerSelect.vue'
import $ from "jquery";

  export default {
       components:{
         editTable,
         customerSelect
       },
        data () {
            return {
               invoice:{},
               contactName:"",
               isCreate:false,
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
            if(this.invoice.isCreate)
            {
              this.isCreate = true;
            }
            this.contactName = this.invoice.contactName;
            if(this.contactName === "Baykarts limited"||this.contactName === "Formula Challenge Limited"||this.contactName === "Pro Karts"||this.contactName === "Daytona Raceway")
            {
              alert("Customer from NewZealand, Gst is $0.00");
            }
            //  if(contact.getContactName().equals("The Kart Centre")||contact.getContactName().equals("Ultimate Karting Sydney"))
             if(this.contactName === "The Kart Centre"||this.contactName === "Ultimate Karting Sydney")
            {
              alert("Customer has 80% discount or free shipping");
            }
          }
        },
        methods: {
          getData () {
          },
          cusNameChanged(){
            console.log($("#cusName input").val());
            this.invoice.contactName =  $("#cusName input").val();
          }
      }// end of methods
    }
</script>
