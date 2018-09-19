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
          
           :default-order-direction="true"
           @cellDataModifiedEvent="onCellDataModifiedEvent"
           @ajaxLoadedEvent="onAjaxLoadedEvent"
           @ajaxLoadingErro="onAjaxLoadingError"
           >
         </vue-bootstrap-table>
       </Col>
       </Row>
       <Row>
       <Col span="24">
         <Form ref="company" :label-width="100">
             <FormItem label="Company Name" prop="opinion">
                 <Input :disabled="hasSubmit" v-model="current.contactName" type="textarea" :autosize="{minRows: 2,maxRows: 5}" placeholder="Plase input contactName name" />
             </FormItem>
             <FormItem label="Contact Person：">
                 <Input :disabled="hasSubmit" v-model="current.personName" type="textarea" :autosize="{minRows: 2,maxRows: 5}" placeholder="Plase input personName " />
             </FormItem>
             <FormItem label="EmailAddress：">
                 <Input :disabled="hasSubmit" v-model="current.emailAddress" type="textarea" :autosize="{minRows: 2,maxRows: 5}" placeholder="Plase input emailAddress" />
             </FormItem>
             <FormItem label="poAddressLine1：">
                 <Input :disabled="hasSubmit" v-model="current.poAddressLine1" type="textarea" :autosize="{minRows: 2,maxRows: 5}" placeholder="Plase input poAddressLine1" />
             </FormItem>
             <FormItem label="poAddressLine2：">
                 <Input :disabled="hasSubmit" v-model="current.poAddressLine2" type="textarea" :autosize="{minRows: 2,maxRows: 5}" placeholder="Plase input poAddressLine2" />
             </FormItem>
             <FormItem label="poAddressLine3：">
                 <Input :disabled="hasSubmit" v-model="current.poAddressLine3" type="textarea" :autosize="{minRows: 2,maxRows: 5}" placeholder="Plase input poAddressLine2" />
             </FormItem>
             <FormItem label="poAddressLine4：">
                 <Input :disabled="hasSubmit" v-model="current.poAddressLine4" type="textarea" :autosize="{minRows: 2,maxRows: 5}" placeholder="Plase input poAddressLine4" />
             </FormItem>
             <FormItem label="City：">
                 <Input :disabled="hasSubmit" v-model="current.poCity" type="textarea" :autosize="{minRows: 2,maxRows: 5}" placeholder="Plase input city" />
             </FormItem>
             <FormItem label="Region：">
                 <Input :disabled="hasSubmit" v-model="current.poRegion" type="textarea" :autosize="{minRows: 2,maxRows: 5}" placeholder="Plase input Region" />
             </FormItem>
             <FormItem label="PostCode：">
                 <Input :disabled="hasSubmit" v-model="current.poPostalCode" type="textarea" :autosize="{minRows: 2,maxRows: 5}" placeholder="Plase input PostCode" />
             </FormItem>
             <FormItem label="Country：">
                 <Input :disabled="hasSubmit" v-model="current.poCountry" type="textarea" :autosize="{minRows: 2,maxRows: 5}" placeholder="Plase input Country" />
             </FormItem>
             <FormItem label="Mobile：">
                 <Input :disabled="hasSubmit" v-model="current.mobile" type="textarea" :autosize="{minRows: 2,maxRows: 5}" placeholder="Plase input mobile" />
             </FormItem>
             <FormItem label="Tel：">
                 <Input :disabled="hasSubmit" v-model="current.phone" type="textarea" :autosize="{minRows: 2,maxRows: 5}" placeholder="Plase input tel" />
             </FormItem>
             <FormItem label="">
                 <Button :disabled="hasSubmit" @click="handleSubmit" style="width:100px;" type="primary">Add</Button>
             </FormItem>
         </Form>
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
              current:{},
              hasSubmit:false,
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
              },
              handleSubmit(){
                this.$http.post(API.host+"/api/contacts/add",this.current, {  headers: {  'Content-Type': 'application/json'  }  }
                    ).then(function(res){
                                // this.data = {};
                                 console.log("success！");
                                  // console.log(res.data.body.message);
                      },function(res){
                                     // this.data = {};
                                           console.log(res.body.message);
                      });
              }
        }
    }
</script>
