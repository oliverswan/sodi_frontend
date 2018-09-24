<template>
<div >
  <v-autocomplete  ref="cusName" style="height:20px" :items="auto_customers"  @blur="blured" :value="initValue" v-model="auto_customer" :get-label="getLabel" :component-item='template' @update-items="updateAutoItems">
    <!-- <div style="clear:both"></div> -->
  </v-autocomplete>
</div>
</template>
<script>
import Autocomplete from 'v-autocomplete'
import CustmerTemplate from './CustmerTemplate.vue'
import API from '../../../api/config'
import $ from "jquery";

export default {
  name: 'customerSelect',
  components: {
      Autocomplete,
      CustmerTemplate,
  },
  props: {
      initValue: {
             type: String,
             default:""
      }
  },
  data () {
       return {
         auto_customer:{},
         auto_customers: [],
         template: CustmerTemplate
       }
  },
  mounted (){
    $("#cusName input").val(this.initValue);
  },
  methods: {
    blured(txt){
      	this.$emit('changed', txt);
    },
    updateAutoItems(text){
      this.$http.get(API.host+"/api/contacts/query/"+text).then(function(res){
                    if(res.data){
                      this.auto_customers = res.data;
                    }
                },function(res){
                  this.loading = false;
                     //alert(res.status)
               });
    },
    getLabel (customer) {
      if(customer)
        return customer.contactName;
    }
  }
}
</script>