<template>
  <div>
       <Table border :columns="columns" :data="data" :loading="loading" style="margin-bottom:5px"></Table>
       <i-button type="primary" icon="plus"  @click.prevent="addNewRule">add Rule</i-button>
  </div>
</template>
<script>
import Cookies from 'js-cookie';
    export default {
        data () {
            return {
                loading:false,
                columns: [
                  {
                      title: 'ID',
                      key: 'id'
                  },
                    {
                        title: 'Name',
                        key: 'name',
                        // render: (h, params) => {
                        //     return h('div', [
                        //         // h('Icon', {
                        //         //     props: {
                        //         //         type: 'person'
                        //         //     }
                        //         // }),
                        //         h('strong', params.row.name)
                        //     ]);
                        // }
                    },
                    {
                        title: 'Enabled',
                        key: 'enable'
                    },
                    {
                        title: 'Action',
                        key: 'action',
                        width: 150,
                        align: 'center',
                        render: (h, params) => {
                            return h('div', [
                                h('Button', {
                                    props: {
                                        type: 'primary',
                                        size: 'small'
                                    },
                                    style: {
                                        marginRight: '5px'
                                    },
                                    on: {
                                        click: () => {
                                            this.edit(params.index)
                                        }
                                    }
                                }, 'Edit'),
                                h('Button', {
                                    props: {
                                        type: 'error',
                                        size: 'small'
                                    },
                                    on: {
                                        click: () => {
                                            this.remove(params.index)
                                        }
                                    }
                                }, 'Delete')
                            ]);
                        }
                    }
                ],
                data: []
            }
        },
          mounted () {
              this.init();
          },
          watch: {
              '$route' () {
                  this.init();
              }
          },
        methods: {
          init () {
              this.loading = true;
              let _this = this;
              let companyid = Cookies.get('usercompanyid');
              this.$http.get("http://13.54.50.156:8080/api/rules/info/"+companyid).then(function(res){
                       if(res.data){
                         this.data = JSON.parse(res.data);
                       }
                      this.loading = false;
                      },function(res){
                                alert(res.status)
             });
            },
            addNewRule(){
              this.$router.push({
                  name: 'ruleeditor'
              });
            },
            edit (index) {
                let ruleid = this.data[index].id;
                let query = {ruleid:ruleid};
                this.$router.push({
                    name: 'ruleeditor',
                    query:query
                });
          },
           remove (index) {
                      this.data6.splice(index, 1);
                  }
      }// end of methods
    }
</script>
