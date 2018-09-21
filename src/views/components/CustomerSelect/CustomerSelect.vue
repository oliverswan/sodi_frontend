<template>

  <v-autocomplete :items="auto_customers" v-model="auto_customer" :get-label="getLabel" :component-item='template' @update-items="updateAutoItems">
    <!-- <div style="clear:both"></div> -->
  </v-autocomplete>

</template>
<script>
import Autocomplete from 'v-autocomplete'
import CustmerTemplate from './CustmerTemplate.vue'
import API from '../../../api/config'


export default {
  name: 'customerSelect',
  components: {
      Autocomplete,
      CustmerTemplate,
  },
  props: {
      // columns: {
      //     type: Array,
      //     required: true,
      // },
      // items: {
      //       type: Array,
      //       required: true,
      // },
      // invoice: {
      //        type: Object,
      //        default: () => ({})
      // }
  },
  data () {
       return {
         auto_customer: {},
         auto_customers: [],
         template: CustmerTemplate
       }
  },
  methods: {

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
