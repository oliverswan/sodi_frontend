<template>
  <div>
    <Card>
          <p slot="title" class="card-title">
              <Icon type="android-checkbox-outline"></Icon>
              ForEach
          </p>
          <a type="text" slot="extra" @click.prevent="add">
             <Icon type="plus"></Icon>  New Action
         </a>
         <a type="text" slot="extra" @click.prevent="close" style="margin-left:10px">
              <Icon type="close-circled"></Icon>
          </a>
          <ticket-action v-for="c in value" :value="c" :key="c.id" @closeAction="deleteAction(c.id)" ></ticket-action>
      </Card>
  </div>
</template>
<script>
import canEditTable from '../tables/canEditTable.vue';
import ticketAction from '../ticket-action/ticket-action.vue';

     export  default {
       name: 'ticketForeach',
       data () {
         return {
           currField: '',// 当前选中的字段
           // fieldlist: ["hours","name","priority"],
           currOperator: ''
         }
       },
       props :{
         value: {
             type: Array,
             default: function () {
                  return [];
              }
         },
         index: {
             type: Number,
             default: ''
         }
       },
       components: {
           // canEditTable,
           ticketAction
       },
       methods: {
         close (){
           this.$emit('closeMe');
         },
         add (){
             this.value.push({"actionname":"","params":""});
         },
         deleteAction (){
           console.log("hello");
         },
         getValue(){
           const result = {actionname:"foreach"};
           result.children = this.value;
           return result;
         },
         addAction(action){
           this.value.push(action);
         }
       }
  }
</script>

<style>
</style>
