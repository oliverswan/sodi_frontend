<template>
  <div>
    <Row>
      <Col>
          
                <p><font size="6" color="red">Proforma Files:</font> </p>
                <div v-for="(url,index) in model.proFormaFileUrls" @click="toDetail(url)">
                    <p>{{url}}</p>
                    <!-- <Button  @click="download(url)" style="width:100px;display:inline" type="primary">download</Button> -->
                    <!-- <a :href="getUrl(item.tel)">图标{{item.tel}}</a> -->
                 </div>
              </Col>
    </Row>
     <Row>
      <Col>
                <p ><font size="6" color="red">Deposit Files: </font></p>
                 <div v-for="(url,index) in model.depositPaymentUrls" @click="toDetail(url)">
                    <p>{{url}}</p>
                    </div>
                  </Col>
    </Row>
    <Row>
      <Col>
                <p ><font size="6" color="red">Balance Files: </font> </p>
                 <div v-for="(url,index) in model.balancePaymentUrls" @click="toDetail(url)">
                    <p>{{url}}</p>
                    </div>
                    </Col>
    </Row>
    <Row>
      <Col>
                <p ><font size="6" color="red">Shipping pre alert Files:  </font></p>
                 <div v-for="(url,index) in model.shippingPreAlertUrls" @click="toDetail(url)">
                    <p>{{url}}</p>
              </div>
            </Col>
    </Row>

    

  </div>
</template>
<script>
// import customerSelect from  '../components/CustomerSelect/CustomerSelect.vue'
import $ from "jquery";
import API from '../../api/config';

  export default {
        data () {
            return {
               model:{},
               isCreate:false,
            }
        },
        created (){
          if(this.$route.params)
          {
            this.model = this.$route.params;
            console.log(this.model.proFormaFileUrls);
            // if(this.invoice.isCreate)
            // {
            //   this.isCreate = true;
            // }
            // this.contactName = this.invoice.contactName;
            // if(this.contactName === "Baykarts limited"||this.contactName === "Formula Challenge Limited"||this.contactName === "Pro Karts"||this.contactName === "Daytona Raceway")
            // {
            //   alert("Customer from NewZealand, Gst is $0.00");
            // }
            // //  if(contact.getContactName().equals("The Kart Centre")||contact.getContactName().equals("Ultimate Karting Sydney"))
            //  if(this.contactName === "The Kart Centre")
            // {
            //   alert("Customer has 80% discount or free shipping");
            // }
          }
        },
        methods: {
          toDetail (url) {
              // console.log(url);
              // alert( API.host+"/potracking/download?url="+url);
               let realurl =  API.host+"/potracking/download?url="+url;
               alert("Download will start,please click ok to continue...");
               this.$http.get(realurl,{'responseType':'blob'}).then(function(res){

                        const content = res.data;
                        const blob = new Blob([content]);
                        let index = url.lastIndexOf("/");
                        const fileName =  url.substring(index+1,url.length);
                        if ("download" in document.createElement("a")) {
                            // 非IE下载
                            const elink = document.createElement("a");
                            elink.download = fileName;
                            elink.style.display = "none";
                            elink.href = URL.createObjectURL(blob);
                            document.body.appendChild(elink);
                            elink.click();
                            URL.revokeObjectURL(elink.href); // 释放URL 对象
                            document.body.removeChild(elink);
                        } else {
                            // IE10+下载
                            navigator.msSaveBlob(blob, fileName);
                        }
                              },function(res){
                                  alert(res.status)
                              });
          }
      }// end of methods
    }
</script>
