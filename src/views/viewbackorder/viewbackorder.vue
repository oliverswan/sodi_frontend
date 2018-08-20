<template>
  <div>
       <Row>
       <Col span="24">
         <Card style="margin:10px" >
                         <p slot="title">{{order.invoiceNumber}}</p>
                         <p v-for="(value, key) in order.orders">{{value}}&nbsp;&nbsp;X&nbsp;&nbsp;{{key}}</p>
                         <i-button type="primary" style="margin:10px"  @click.prevent="complete(order.invoiceNumber)">Complete</i-button>
         </Card>
       </Col>
       </Row>
  </div>
</template>
<script>
    export default {
        data () {
            return {
              order:{}
            }
        },
        mounted (){
          if(this.$route.params)
          {
            this.order = this.$route.params;
            console.log(this.order);
          }
        },
        methods: {
          complete(id)
          {
            this.$http.get(API.host+"/api/backorders/complete/"+this.order.id).then(function(res){
                          // this.getData ();
                      },function(res){
                     });
          }
      }// end of methods
    }
</script>
