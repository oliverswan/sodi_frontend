<style lang="less">
    @import './login.less';
</style>

<template>
    <div class="sys-login" @keydown.enter="handleSubmit">
        <div class="login-area">
          <div class="logo">
               <img src="../images/sodi_logo.svg" alt="">
          </div>
          <div class="form-con">
              <Form ref="loginForm" :model="form" :rules="rules">
                  <FormItem prop="userName">
                      <Input v-model="form.userName" placeholder="请输入用户名">
                          <span slot="prepend">
                              <Icon :size="16" type="person"></Icon>
                          </span>
                      </Input>
                  </FormItem>
                  <FormItem prop="password">
                      <Input type="password" v-model="form.password" placeholder="请输入密码">
                          <span slot="prepend">
                              <Icon :size="14" type="locked"></Icon>
                          </span>
                      </Input>
                  </FormItem>
                  <!-- <FormItem prop="password">
                      <Input type="text"   v-if="captcha" >
                          <span slot="prepend">
                              <Icon :size="14" type="locked"></Icon>
                          </span>
                      </Input>
                      <img src="" alt="">
                  </FormItem> -->
                  <FormItem>
                      <Button @click="handleSubmit" type="primary" long>登录</Button>
                  </FormItem>
              </Form>
            <a class="textR">forget password？</a>
          </div>

        </div>
    </div>
</template>

<script>
import Cookies from 'js-cookie';
export default {
    data () {
        return {
            form: {
                userName: 'oliver',
                password: ''
            },
            rules: {
                userName: [
                    { required: true, message: 'Must input username', trigger: 'blur' }
                ],
                password: [
                    { required: true, message: 'Must input password', trigger: 'blur' }
                ]
            }
        };
    },
    methods: {
      handleSubmit () {
          this.$refs.loginForm.validate((valid) => {
                 this.$router.push({name: 'home_index' });
                 if (valid) {
                    Cookies.set('user', this.form.userName);
                    Cookies.set('password', this.form.password);
                    this.$store.commit('setAvator', 'https://ss1.bdstatic.com/70cFvXSh_Q1YnxGkpoWK1HF6hhy/it/u=3448484253,3685836170&fm=27&gp=0.jpg');
                    // if (this.form.userName === 'iview_admin') {
                    //     Cookies.set('access', 0);
                    // } else {
                    //     Cookies.set('access', 1);
                    // }
                                           Cookies.set('usercompanyid', "0001");
                                           this.$store.commit('switchLang', 'en-US');
                                           this.$store.commit('changeMenuTheme', 'dark');
                                           this.$store.commit('changeMainTheme', 'b');
                                           this.$router.push({name: 'home_index' });
                    // ===============================================================================
                    // const userUrl = 'http://127.0.0.1:8083/api/users/'+this.form.userName;
                    // this.$http.get(userUrl).then(function(res){
                    //          if(res.data){
                    //            if(res.data.password === this.form.password){
                    //                         // 登录成功  切换页面
                    //                         // Cookies.set('username', res.data.name);
                    //                         Cookies.set('usercompanyid', res.data.companyId);
                    //                         // console.log('save companyId:'+res.data.companyId);
                    //                         this.$store.commit('switchLang', 'en-US');
                    //                         this.$store.commit('changeMenuTheme', 'light');
                    //                         this.$store.commit('changeMainTheme', 'b');
                    //                         this.$router.push({name: 'home_index' });
                    //             }else{
                    //                         alert("login_failed");
                    //               }
                    //          }
                    // },function(res){
                    //                       // alert(res.status)
                    //                   });
                    //=============================================================================

                 }
          });

        }
    }
};
</script>

<style>

</style>
