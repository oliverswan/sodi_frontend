<template>
  <div>
    <Row>
        <Col span="5">
          <Select  ref="field"  v-model="value.field">
              <Option v-for="item in options.field" :value="item" :key="item">{{ item }}</Option>
          </Select>
        </Col>
        <Col span="5" style="padding-left:10px">
          <Select  ref="operator"  v-model="value.operator">
              <Option v-for="item in options.conditionoperator" :value="item" :key="item">{{ item }}</Option>
          </Select>
        </Col>
        <Col span="5" style="padding-left:10px">
          <Input  placeholder="please input value " v-model="value.cvalue" ></Input>
        </Col>
        <Col span="5" style="padding-left:10px">
          <Select  ref="connector"  v-model="value.connector">
              <Option v-for="item in options.connector" :value="item" :key="item">{{ item }}</Option>
          </Select>
        </Col>

        <Col span="4">
          <a type="text" @click.prevent="close" style="margin-left:10px">
               <Icon type="close-circled"></Icon>
           </a>
        </Col>
    </Row>
  </div>
</template>
<script>
     export  default {
       name: 'ticketCondition',
       data () {
         return {
           options:{
             field: ["hours","name","priority"],
             conditionoperator: ["=","contains","!=",">","<"],
             connector: ["AND","OR",""]
           }
         }
       },
       props: {
         value:{
           field: {
               type: String,
               default: ''
           },
           operator: {
               type: String,
               default: ''
           },
           cvalue: {
               type: String,
               default: ''
           },
           connector: {
               type: String,
               default: ''
           }
         }
       },
       mounted(){
         this.getData();
       },
       methods: {
         close () {
             this.$emit('closeCondition');
         },
         getData() {
           if(!window.localStorage.ruleoptions)
           {
                   console.log("Request options...");
                   let _this = this;
                   this.$http.get('http://13.54.50.156:8080/api/dicts').then(function(res){
                                 if(res.data)
                                 {
                                     res.data.forEach(function (dict) {
                                        _this.options[dict.name] = dict.values.split(",");
                                    });
                                    if(window.localStorage)
                                    {

                                      window.localStorage.setItem("ruleoptions", JSON.stringify(_this.options));
                                        // alert(window.localStorage.ruleoptions);
                                    }else{
                                      Cookies.set("ruleoptions", JSON.stringify(_this.options));
                                    }
                                 }
                  },function(res){
                                       alert(res.status)
                  });
           }else{
               let localoptions = JSON.parse(window.localStorage.ruleoptions);
               this.options.field = localoptions.field;
               this.options.conditionoperator = localoptions.conditionoperator;
               this.options.connector = localoptions.connector;
              }
         }
       }
     }
</script>
<style>
</style>
