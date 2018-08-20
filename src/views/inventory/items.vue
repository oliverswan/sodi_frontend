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
                 url: "http://127.0.0.1:8083/api/items?echo=1",
                 method: "GET",
                 delegate: false
               },
              columns:[{
                  title: 'id',
                  visible: true,
              },
              {
                  title: 'code',
                  visible: true,
                  editable: true,
              },
              {
                title: 'name',
                visible: true,
                editable: true,
              },
              {
                title: 'stock',
                visible: true,
                editable: true,
              },
              {
                title: 'soldThisYear',
                visible: true,
                editable: true,
              },
              {
                title: 'spm',
                visible: true,
                editable: true,
              },
              {
                title: 'msoh',
                visible: true,
                editable: true,
              },
              {
                title: 'weight',
                visible: true,
                editable: true,
              }
              ]
            }
        },

        methods:{
              onCellDataModifiedEvent(originalValue, newValue, columnTitle, entry){
                console.log(API.host);
                this.$http.post(API.host+"/api/items/save",entry, {  headers: {  'Content-Type': 'application/json'  }  }
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
