<template>
  <div>
       <Row>
       <Col span="24">
         <vue-bootstrap-table
           :columns="columns"
           :values="data"
           :show-filter="true"
           :sortable="true"
           :ajax="aj"
           :paginated="true"
           :filter-case-sensitive="false"
           :default-order-column="code"
           :default-order-direction="true"
           @cellDataModifiedEvent="onCellDataModifiedEvent"
           @ajaxLoadedEvent="onAjaxLoadedEvent"
           @ajaxLoadingErro="onAjaxLoadingError"
           >
         </vue-bootstrap-table>
       </Col>
       </Row>
  </div>
</template>
<script>
import API from '../../api/config';
import VueBootstrapTable from '../components/VueBootstrapTable.vue';

    export default {
        components: {
            VueBootstrapTable
        },
        data () {
            return {
              data:[],
              aj: {
                 enabled: true,
                 url: API.host+"/api/contacts?echo=2",
                 method: "GET",
                 delegate: false
               },
              columns:[{
                  title: 'id',
                  visible: true,
              },
              {
                  title: 'contactName',
                  visible: true,
                  editable: true,
              },
              {
                title: 'personName',
                visible: true,
                editable: true,
              },
              {
                title: 'emailAddress',
                visible: true,
                editable: true,
              },
              {
                title: 'poAddressLine1',
                visible: true,
                editable: true,
              },
              {
                title: 'poAddressLine2',
                visible: true,
                editable: true,
              },
              {
                title: 'poAddressLine3',
                visible: true,
                editable: true,
              },
              {
                title: 'poAddressLine4',
                visible: true,
                editable: true,
              },
              {
                title: 'poCity',
                visible: true,
                editable: true,
              }
              ,
              {
                title: 'poRegion',
                visible: true,
                editable: true,
              },
              {
                title: 'poPostalCode',
                visible: true,
                editable: true,
              },
              {
                title: 'poCountry',
                visible: true,
                editable: true,
              },
              {
                title: 'mobile',
                visible: true,
                editable: true,
              },
              {
                title: 'phone',
                visible: true,
                editable: true,
              }
              ]
            }
        },

        methods:{
              onCellDataModifiedEvent(originalValue, newValue, columnTitle, entry){
                this.$http.post(API.host+"/api/contacts/update",entry, {  headers: {  'Content-Type': 'application/json'  }  }
                    ).then(function(res){
                                // this.data = {};
                                 console.log("success！");
                                  // console.log(res.data.body.message);
                      },function(res){
                                     // this.data = {};
                                           console.log(res.body.message);
                      });

                  console.log(entry);
              },
              onAjaxLoadedEvent( data ) {
                  console.log("ajaxLoadedEvent - data : " + data );
              },
              onAjaxLoadingError( data ) {
                  console.log("ajaxLoadedEvent - data : " + data );
              }
        }
    }
</script>
