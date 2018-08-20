<template>
  <div>
       <Row>
       <Col span="24">
               <can-edit-table
                   refs="dictTable"
                   v-model="data"
                   @on-cell-change="handleCellChange"
                   @on-change="handleChange"
                   :editIncell="true"
                   :columns-list="columns"
               ></can-edit-table>
       </Col>
       </Row>
  </div>
</template>
<script>
import canEditTable from '../tables/components/canEditTable.vue';

    export default {
      components: {
          canEditTable
      },
        data () {
            return {
              data:[],
              columns:[{
                  title: 'Seq',
                  type: 'index',
                  width: 80,
                  align: 'center'
              },
              {
                  title: 'Name',
                  align: 'center',
                  key: 'name'
              },
              {
                  title: 'Value',
                  align: 'center',
                  key: 'values',
                  editable: true
              },
              {
                  title: 'Operation',
                  align: 'center',
                  width: 120,
                  key: 'handle',
                  handle: ['delete']
              }]
            }
        },
        mounted () {
            this.getData();
        },
        methods: {
          getData () {
            // this.data =[{"name":"field","values":"\"aaaa\",\"bbbb\",\"cccc\",\"dddddddd\""}];
            // this.loading = true;
           //  let _this = this;
          this.$http.get("http://13.54.50.156:8080/api/dicts").then(function(res){
                         if(res.data){
                           // console.log(res.data);
                           this.data = res.data;
                         }
                     },function(res){
                          alert(res.status)
                    });
          },
            // handleNetConnect (state) {
            //     this.breakConnect = state;
            // },
            // handleLowSpeed (state) {
            //     this.lowNetSpeed = state;
            // },
            handleDel (val, index) {
                this.$Message.success('删除了第' + (index + 1) + '行数据');
            },
            handleCellChange (val, index, key) {
                // this.$Message.success('修改了第 ' + (index) + ' 行列名为 ' + key + ' 的数据');
                alert(this.data[0].values);
                // this.$Message.success(this.data[index].values);
            },
            handleChange (val, index) {
                this.$Message.success('修改了第' + (index + 1) + '行数据');
            }
      }// end of methods
    }
</script>
