<template>
  <div>
        <Row>
          <B>Reorder Report: </B>&nbsp;&nbsp;&nbsp;&nbsp;
          <Input v-model="month" placeholder="Enter month for reorder..." style="width: 300px" />
          <i-button type="primary" style="margin:10px"  @click.prevent="report">Generate</i-button>
         </Row>
         <Row>
           <B>Back Order Report (base on itemCode): </B>&nbsp;&nbsp;&nbsp;&nbsp;
           <i-button type="primary" style="margin:10px"  @click.prevent="report_backorder">Generate</i-button>
         </Row>
         <Row>
           <B>Back Order Report (base on customer): </B>&nbsp;&nbsp;&nbsp;&nbsp;
           <i-button type="primary" style="margin:10px"  @click.prevent="report_delivery">Generate</i-button>
          </Row>
           <Row>
           <B>Monthly sales Report: </B>&nbsp;&nbsp;&nbsp;&nbsp;
           <Input v-model="monthlySales" placeholder="Enter month for sales history..." style="width: 300px" />
           <i-button type="primary" style="margin:10px"  @click.prevent="report_monthlysales">Generate</i-button>
          </Row>
  </div>
</template>
<script>
import API from '../../api/config';

    export default {
        data () {
            return {
              month:0,
              monthlySales:0
            }
        },
        methods: {
          async download(url){
            let response = await Vue.http.get(url, {responseType: 'arraybuffer'});
            let blob = new Blob([response.data], {type:response.headers.get('content-type')});
            let link = document.createElement('a');
            link.href = window.URL.createObjectURL(blob);
            link.download = this.name;
            link.click();
          },
          showFile(blob){
            var newBlob = new Blob([blob], {type: "application/pdf"})
              if (window.navigator && window.navigator.msSaveOrOpenBlob) {
                window.navigator.msSaveOrOpenBlob(newBlob);
                return;
              }
              const data = window.URL.createObjectURL(newBlob);
              var link = document.createElement('a');
              link.href = data;
              link.download="reorder.pdf";
              link.click();
              // setTimeout(function(){ window.URL.revokeObjectURL(data);
              // , 100}
          },
            report() {
              let url = API.host+"/api/reports/reorder/" + this.month;
              window.open(url);
            }
            ,report_backorder() {
              let url = API.host+"/api/reports/backorder";
              window.open(url);
            }  ,report_delivery() {
                let url = API.host+"/api/reports/backorderDelivery";
                window.open(url);
            },report_monthlysales(){
               let url = API.host+"/api/reports/salehistory/" + this.monthlySales;
              window.open(url);
            }

      }// end of methods
    }

</script>
