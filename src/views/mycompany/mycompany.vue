<template>
  <div>
    <Row>
        <Card>
          <!-- class="step-header-con"
            <div >
                <h3>Title</h3>
                <h5>SubTitle</h5>
            </div>-->
            <!-- v-html="stepData.content" class="step-content"-->
            <Form ref="company" :label-width="100">
                <FormItem label="Company Name" prop="opinion">
                    <Input :disabled="hasSubmit" v-model="companyData.companyname" type="textarea" :autosize="{minRows: 2,maxRows: 5}" placeholder="Plase input comapny name" />
                </FormItem>
                <FormItem label="Public Key：">
                    <Input :disabled="hasSubmit" v-model="companyData.publickey" type="textarea" :autosize="{minRows: 2,maxRows: 5}" placeholder="Plase input comapny public key" />
                </FormItem>
                <FormItem label="Private Key：">
                    <Input :disabled="hasSubmit" v-model="companyData.privatekey" type="textarea" :autosize="{minRows: 2,maxRows: 5}" placeholder="Plase input comapny private key" />
                </FormItem>
                <FormItem label="Base Path：">
                    <Input :disabled="hasSubmit" v-model="companyData.base_path" type="textarea" :autosize="{minRows: 2,maxRows: 5}" placeholder="Plase input comapny base path" />
                </FormItem
                <FormItem label="">
                    <Button :disabled="hasSubmit" @click="handleSubmit" style="width:100px;" type="primary">Save</Button>
                </FormItem>
            </Form>

        </Card>
    </Row>
  </div>
</template>
<script>
export default {
  data () {
      return {
          companyData: {
              companyname: '',
              publickey: '',
              privatekey: '',
              base_path:''
          },
          hasSubmit: false,
          status: 'wait'
      };
  },
  methods: {
      handleSubmit () {
          this.$refs['company'].validate((valid) => {
              if (valid) {
                  this.hasSubmit = true;
              }
          });
      }
  },
  mounted () {
      this.$http.get('http://13.54.50.156:8080/api/company/0').then(function(res){
                            if(res.data)
                            {
                              this.companyData.companyname = res.data.companyname;
                              this.companyData.publickey = res.data.publickey;
                              this.companyData.privatekey = res.data.privatekey;
                              this.companyData.base_path = res.data.base_path;
                            }
                        },function(res){
                            // alert(res.status)
                        });
      this.companyData = {
        companyname: '',
        publickey: '',
        privatekey: '',
        base_path:''
      }
  }
};
</script>
