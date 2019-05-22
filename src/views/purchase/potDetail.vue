<template>
  <div>
    <Row>
      <Col>
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

                <b>customerName:   </b><Input v-model="model.customerName" style="width: 20%"></Input><br><br>
                <b>partsType:   </b><Input v-model="model.partsType" style="width: 20%"></Input><br><br>
                <b>date:   </b><Input v-model="model.date" style="width: 20%"></Input><br><br>
                <b>proFormaNumber:   </b><Input v-model="model.proFormaNumber" style="width: 20%"></Input><br><br>
                <b>productionDate:   </b><Input v-model="model.productionDate" style="width: 20%"></Input><br><br>
                <i-button type="primary" style="margin:10px"  @click.prevent="submit">Submit</i-button>
           
      </Col>
    </Row>
  </div>
</template>
<script>
import API from '../../api/config';
import $ from "jquery";

  export default {
       components:{
       },
      data () {
            return {
               model:{}
            }
        },
        methods: {
          getData () {
          },
          submit(){
              this.$http.post(API.host+"/potracking/save",this.model, {  headers: {  'Content-Type': 'application/json'  }  }
                    ).then(function(res){
                                // this.data = {};
                                 alert("success！");
                                  // console.log(res.data.body.message);
                      },function(res){
                                     // this.data = {};
                                           console.log(res.body.message);
                      }); 
          }
      }// end of methods
    }
</script>
