<style lang="less">
    @import "./home.less";
    @import "../../styles/common.less";
</style>
<template>
    <div class="home-main">
         <!-- <Row>
                 <i-col :xs="12" :md="8" :lg="4" style="height: 120px;padding-bottom: 10px;">
                        <infor-card shadow :color="infor.color" :icon="infor.icon" :icon-size="36">
                        <count-to :end="infor.count" :decimals="2" count-class="count-style"/>
                        <p>{{ infor.title }}</p>
                        </infor-card>
                    </i-col>
                    <i-col :xs="12" :md="8" :lg="4" style="height: 120px;padding-bottom: 10px;">
                        <infor-card shadow :color="infor.color" :icon="infor.icon" :icon-size="36">
                        <count-to :end="infor.count" count-class="count-style"/>
                        <p>{{ infor.title }}</p>
                        </infor-card>
                    </i-col>
                    <i-col :xs="12" :md="8" :lg="4" style="height: 120px;padding-bottom: 10px;">
                        <infor-card shadow :color="infor.color" :icon="infor.icon" :icon-size="36">
                        <count-to :end="infor.count" count-class="count-style"/>
                        <p>{{ infor.title }}</p>
                        </infor-card>
                    </i-col>
                </Row> -->
                <Row :gutter="20">
                <i-col :xs="12" :md="8" :lg="4" v-for="(infor, i) in inforCardData" :key="`infor-${i}`" style="height: 120px;padding-bottom: 10px;">
                    <infor-card shadow :color="infor.color" :icon="infor.icon"   :icon-size="36">
                    <count-to :end="infor.count" :decimals="2" count-class="count-style"/>
                    <p>{{ infor.title }}</p>
                    </infor-card>
                </i-col>
    </Row>
        <Row :gutter="10">

            <Col :md="24" :lg="8"> 
                <Row class-name="home-page-row1" :gutter="10">
                    <!-- <Col :md="12" :lg="24" :style="{marginBottom: '10px'}">
                        <Card>
                            <Row type="flex" class="user-infor">
                                <Col span="8">
                                    <Row class-name="made-child-con-middle" type="flex" align="middle">
                                        <img class="avator-img" :src="avatorPath" />
                                    </Row>
                                </Col>
                                <Col span="16" style="padding-left:6px;">
                                    <Row class-name="made-child-con-middle" type="flex" align="middle">
                                        <div>
                                            <b class="card-user-infor-name">Admin</b>
                                            <p>super admin</p>
                                        </div>
                                    </Row>
                                </Col>
                            </Row>
                            <div class="line-gray"></div>
                            <Row class="margin-top-8">
                                <Col span="8"><p class="notwrap">Last login time:</p></Col>
                                <Col span="16" class="padding-left-8">2018.08.12-13:32:20</Col>
                            </Row>
                            <Row class="margin-top-8">
                                <Col span="8"><p class="notwrap">Last login location:</p></Col>
                                <Col span="16" class="padding-left-8">Melbourne</Col>
                            </Row>
                        </Card>
                       
                    </Col> -->
                  
                    <Col :md="12" :lg="24" :style="{marginBottom: '10px'}">
                       <Card>
                            <p slot="title" class="card-title">
                                <Icon type="android-checkbox-outline"></Icon>
                                ToDo List
                            </p>
                            <a type="text" slot="extra" @click.prevent="addNewToDoItem">
                                <Icon type="plus-round"></Icon>
                            </a>
                            <Modal
                                v-model="showAddNewTodo"
                                title="Add new todo"
                                @on-ok="addNew"
                                @on-cancel="cancelAdd">
                                <Row type="flex" justify="center">
                                    <Input v-model="newToDoItemValue" icon="compose" placeholder="Please Input..." style="width: 300px" />
                                </Row>
                                <Row slot="footer">
                                    <Button type="text" @click="cancelAdd">Cancel</Button>
                                    <Button type="primary" @click="addNew">Confirm</Button>
                                </Row>
                            </Modal>
                            <div class="to-do-list-con">
                                <div v-for="(item, index) in toDoList" :key="'todo-item' + (toDoList.length - index)" class="to-do-item">
                                    <to-do-list-item :todo="item"  @finish="check" ></to-do-list-item>
                                </div>
                            </div>
                        </Card> 
                        <sales-chart></sales-chart>
                    </Col>
                </Row>
            </Col>
        </Row>
    </div>
</template>

<script>
import cityData from './map-data/get-city-value.js';
import homeMap from './components/map.vue';
import dataSourcePie from './components/dataSourcePie.vue';
import visiteVolume from './components/visiteVolume.vue';
import serviceRequests from './components/serviceRequests.vue';
import userFlow from './components/userFlow.vue';
import countUp from './components/countUp.vue';

import mapDataTable from './components/mapDataTable.vue';
import toDoListItem from './components/toDoListItem.vue';

import InforCard from './components/infor-card.vue';
import CountTo from './components/count-to/count-to.vue';

import salesChart from '../components/salesChart'
import API from '../../api/config';
export default {
    name: 'home',
    components: {
        homeMap,
        dataSourcePie,
        visiteVolume,
        serviceRequests,
        userFlow,
        countUp,
        mapDataTable,
        toDoListItem,
        InforCard,
        CountTo,
        salesChart
    },
    data () {
        return {
            toDoList: [
                // {
                //     title: 'do something1'
                // },
                // {
                //     title: 'do something2'
                // }
            ],
            count: {
                createUser: 496,
                visit: 3264,
                collection: 24389305,
                transfer: 39503498
            },
             inforCardData: [
        { title: 'Total Value', icon: 'social-usd', count: 0, color: '#2d8cf0' },
        { title: 'Meakone Value', icon: 'md-locate', count:  0, color: '#19be6b' },
        { title: 'Maxpower Value', icon: 'md-help-circle', count:  0, color: '#ff9900' }
        // ,
        // { title: '分享统计', icon: 'md-share', count: 657, color: '#ed3f14' },
        // { title: '新增互动', icon: 'md-chatbubbles', count: 12, color: '#E46CBB' },
        // { title: '新增页面', icon: 'md-map', count: 14, color: '#9A66E4' }
      ],
            // infor:   { title: 'Inventory Value', icon: 'md-person-add', count: 10.333, color: '#2d8cf0' },
            cityData: cityData,
            showAddNewTodo: false,
            newToDoItemValue: '',
            currentTodo:{}
        };
    },
    computed: {
        avatorPath () {
            return localStorage.avatorImgPath;
        }
      
    },
    beforeCreate: function () {
           this.$http.get(API.host+"/api/items/statistic").then(function(res){
                  if(res.data){
                      console.log(res.data);
                      this.inforCardData[0].count = res.data.totalvalues;
                      this.inforCardData[1].count = res.data.meakonevalues;
                      this.inforCardData[2].count = res.data.maxpowervalues;
                  }
              },function(res){
             });

              this.$http.get(API.host+"/api/todo").then(function(res){
                  if(res.data){
                      console.log(res.data);
                      this.toDoList = res.data;
                  }
              },function(res){
             });
        },
    methods: {
        addNewToDoItem () {
            this.showAddNewTodo = true;
        },
        
/** 
* 将数值格式化成金额形式 
* 
* @param num 数值(Number或者String) 
* @param precision 精度，默认不变
* @param separator 分隔符，默认为逗号
* @return 金额格式的字符串,如'1,234,567'，默认返回NaN
* @type String 
*/
 formatNumber(num, precision, separator) {
    var parts;
    // 判断是否为数字
    if (!isNaN(parseFloat(num)) && isFinite(num)) {
        // 把类似 .5, 5. 之类的数据转化成0.5, 5, 为数据精度处理做准, 至于为什么
        // 不在判断中直接写 if (!isNaN(num = parseFloat(num)) && isFinite(num))
        // 是因为parseFloat有一个奇怪的精度问题, 比如 parseFloat(12312312.1234567119)
        // 的值变成了 12312312.123456713
        num = Number(num);
        // 处理小数点位数
        num = (typeof precision !== 'undefined' ? (Math.round(num * Math.pow(10,precision)) / Math.pow(10,precision)).toFixed(precision) : num).toString();
        // 分离数字的小数部分和整数部分
        parts = num.split('.');
        // 整数部分加[separator]分隔, 借用一个著名的正则表达式
        parts[0] = parts[0].toString().replace(/(\d)(?=(\d{3})+(?!\d))/g, '$1' + (separator || ','));
 
        return parts.join('.');
    }
    return NaN;
},
        addNew () {
            if (this.newToDoItemValue.length !== 0) {
                this.currentTodo = {
                    "title": this.newToDoItemValue
                };

                // 本地添加
                this.toDoList.unshift(this.currentTodo);

                // 数据库添加
                 this.$http.post(API.host+"/api/todo/save",this.currentTodo, {  headers: {  'Content-Type': 'application/json'  }  }
                    ).then(function(res){
                                // this.data = {};
                                 alert("success！");
                                  // console.log(res.data.body.message);
                      },function(res){
                                     // this.data = {};
                                           console.log(res.body.message);
                      }); 

                setTimeout(() => {
                    this.newToDoItemValue = '';
                }, 200);
                this.showAddNewTodo = false;
            } else {
                this.$Message.error('Please input what to do...');
            }
        },
        cancelAdd () {
            this.showAddNewTodo = false;
            this.newToDoItemValue = '';
        },check(para){
                
                 this.$http.post(API.host+"/api/todo/save",para, {  headers: {  'Content-Type': 'application/json'  }  }
                    ).then(function(res){
                                // this.data = {};
                                 alert("success！");
                                  // console.log(res.data.body.message);
                      },function(res){
                                     // this.data = {};
                                           console.log(res.body.message);
                      }); 

        }
    }
};
</script>

<style lang="less">
.count-style{
  font-size: 20px;
}
</style>
