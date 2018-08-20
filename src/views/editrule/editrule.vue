<template>
  <div style="background:#fff">

    <div style="margin:10px 0px;padding:10px">
         <i-button type="primary" icon="ios-loop"  @click.prevent="addNewForeach" >add foreach</i-button>
         <i-button type="primary" icon="ios-gear-outline"  @click.prevent="addNewAction">add action</i-button>
         <i-button type="error" shape="circle" @click.prevent="save" style="float:right">Save</i-button>
    </div>
    <Row >
      <!-- col来定义宽度 使用css  height来定义高度 -->
      <Col span="24">
        <Card>
              <p slot="title" class="card-title">
                  Rule Properties
              </p>
              <p >
                   <b>Rule name:</b>:
                   <Input v-model="rule.name" placeholder="Enter something..." style="width: 300px"></Input>
              </p>
              <p >
                   <b>Enable:</b>
                     <i-switch :value="rule.enable" @on-change="changeEnable" size="large"></i-switch>
              </p>
          </Card>
        </Col>
    </Row>

    <Row >
      <!-- col来定义宽度 使用css  height来定义高度 -->
      <Col span="24">
        <Card>
              <p slot="title" class="card-title">
                  <Icon type="android-checkbox-outline"></Icon>
                  Conditions
              </p>
               <a type="text" slot="extra" @click.prevent="addNewCondition">
                  <Icon type="plus-round"></Icon> New Condition
              </a>
              <!-- 条件列表 -->
              <ticket-condition v-for="c in rule.conditions" :value="c" :key="c.id" @closeCondition="deleteCondition(c.id)" ></ticket-condition>
          </Card>
        </Col>
    </Row>

    <Row class="margin-top-10">
      <Col span="24">
        <!-- :text="element.text" :data="element.data" -->
         <div v-for="element in elements" ref="nodes" :is="element.type" :index="element.index"  :value="element.value" @closeMe="deleteChild(element.index)"></div>
      </Col>
    </Row>

  </div>
</template>
<script>
import ticketCondition from '../ui-components/ticket-condition/ticket-condition.vue';
import ticketForeach from '../ui-components/ticket-foreach/ticket-foreach.vue';
import ticketAction from '../ui-components/ticket-action/ticket-action.vue';
import Util from '../../libs/util';
import Cookies from 'js-cookie';

export default {
  data () {
    return {
      ruleStatusList: ["enable","disable"],
      elements:[],
      rule:{
        conditions:[],
        actions:[]
      }
    }
  },
  components: {
      ticketCondition,
      ticketForeach,
      ticketAction
    },
    mounted (){
      if(this.$route.query.ruleid)
      {
          this.init(this.$route.query.ruleid);
      }
    },
  methods: {
      init(id){
        // 获取rule详情
        this.$http.get('http://13.54.50.156:8080/api/rules/'+id).then(function(res){
                              if(res.data)
                              {
                                this.rule = JSON.parse(res.data);
                                this.parseRule();
                              }
                          },function(res){
                              // alert(res.status)
                          });
      },
      parseRule(){
           // this.conditions = this.rule.conditions;
           var _this = this;
           this.rule.actions.forEach(function (action) {
              if(action.actionname === "foreach")
              {
                _this.addNewForeach(null,action.children);
              }
          });

      },
      addNewCondition () {
        this.rule.conditions.push({"field":"","operator":"","cvalue":"","connector":""});
      },
      addNewForeach (event,children) {
        const i = this.elements.length;
        const id = "foreach_"+i;
        const obj = { 'type': 'ticket-foreach',index:i,name:id,value:children};
        this.elements.push(obj);
      },
      deleteChild(index)
      {
        // console.log(this.$refs['element.name'][0].actionList.length);
        this.elements.splice(index,1);
      },
      save()
      {
        //0. 收集collections
        const rule = {};
        // this.rule.conditions = this.conditions;
        this.rule.actions = [];
        this.rule.companyid = Cookies.get('usercompanyid');
        //1.收集actions
        if(this.$refs.nodes){
          for(var i=0;i<this.$refs.nodes.length;i++)
          {
            this.rule.actions.push(this.$refs.nodes[i].getValue());
          }
        }
        this.$http.post('http://13.54.50.156:8080/api/rules/save',this.rule).then(function(res){
                              alert(res.data);
                          },function(res){
                              // alert(res.status)
                          });
        // console.log(JSON.stringify(rule));
      },
      deleteCondition(id)
      {
        for(var i=0;i<this.rule.conditions.length;i++)
        {
            if(this.rule.conditions[i].id == id)
            {
              this.rule.conditions.splice(i,1);
              return;
            }
        }
      },
      addNewAction () {
        const i = this.elements.length;
        const id = "action_"+i;
        const action = {'type': 'ticket-action',index:i,name:id,value:{"actionname":"","params":""}};
        this.elements.push(action);
      },
      handleCellChange (val, index, key) {
          this.$Message.success('修改了第 ' + (index + 1) + ' 行列名为 ' + key + ' 的数据');
      },
      handleChange (val, index) {
          this.$Message.success('修改了第' + (index + 1) + '行数据');
      },
      changeEnable (res) {
        this.rule.enable = res;
      }
    }
};
</script>
<style>
</style>
