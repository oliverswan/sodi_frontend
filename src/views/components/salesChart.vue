<script>
// CommitChart.js
import { Bar } from 'vue-chartjs'
import API from '../../api/config';

export default {
  extends: Bar,

  data () {
      return {
        labels:[],
        datasets:[]
      };
  },
  mounted () {
    // Overwriting base render method with actual data.
    // this.getData();
    let that = this;
    this.$http.get(API.host+"/api/items/salesmost/10").then(function(res){
                  if(res.data){
                    // a = res.data.labels;
                    // b = res.data.datasets;
                    that.renderChart({
                          labels:res.data.labels,
                          datasets: [
                            {
                              label: 'Sales Quantity Top 10',
                              backgroundColor: '#f87979',
                              data: res.data.datasets
                            }
                          ]
                        });
                  }
              },function(res){
             });



    // this.renderChart({
    //       labels:a,
    //       datasets: [
    //         {
    //           label: 'GitHub Commits',
    //           backgroundColor: '#f87979',
    //           data: b
    //         }
    //       ]
    //     });

  },
  methods :{
      getData(){
        this.$http.get(API.host+"/api/items/salesmost/10").then(function(res){
                      if(res.data){
                        this.labels = res.data.labels;
                        this.datasets = res.data.datasets;
                      }
                  },function(res){
                 });
      }
  }
}
</script>
