<template>
  <div style="margin:3px 0px">
    <Card>
    <Row>
        <Col span="5">
          <Select  ref="actionname"  v-model="value.actionname">
              <Option v-for="item in options.actionnames" :value="item" :key="item">{{ item }}</Option>
          </Select>
        </Col>
        <Col span="15" style="padding-left:10px" >
           <Input v-model="value.params" type="textarea" :rows="4" placeholder="Enter something..."></Input>
        </Col>

        <Col span="4">
          <a type="text" @click.prevent="close" style="margin-left:10px">
               <Icon type="close-circled"></Icon>
           </a>
        </Col>
    </Row>
  </Card>
  </div>
</template>
<script>
export  default {
  name: 'ticketAction',
  data () {
    return {
      options:{
        actionnames: []
      }
    }
  },
  props: {
    value:{
      actionname: {
          type: String,
          default: ''
      },
      params: {
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
        this.$emit('closeAction');
    },
     getValue(){
       return this.value;
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
               this.options.actionnames = localoptions.actionnames;
          }
     }
  }
}
</script>
<style>
</style>
