<template>
  <div id="app_login">
    <div class="content-bg"></div>
    <div class="content">
      <div class="login">
        <el-form :model="ruleForm2" :rules="rules2" ref="ruleForm2" label-position="left" label-width="0px">
          <div class="login-form">
            <h3 class="title" style="margin-top: 80px">
              <img src="../assets/logo2.png" alt="">
            </h3>
            <el-form-item prop="account" style="margin-top: 100px">
              <div class="el-div">
                <el-span style="width: 44px;height: 44px;">
                  <img src="../assets/loginmine_icon.png" style="margin-left: 9px;margin-top: 8px;" alt="">
                </el-span>
                <el-span style="display: inline-block;width: 1px; height: 41px;left:36px;top:1px;z-index:1;background: #BFC2CA;position: absolute;top:0px"></el-span>
                <el-input type="text" style="width: 209px;margin-left: 5px" v-model="ruleForm2.account" auto-complete="off" placeholder="输入账号" @keyup.enter.native="handleSubmit2"></el-input>
              </div>

            </el-form-item>
            <el-form-item prop="checkPass">
              <div class="el-div">
                <el-span style="width: 44px;height: 44px;">
                  <img src="../assets/loginpassword_icon.png" style="margin-left: 9px;margin-top: 8px;" alt="">
                </el-span>
                <el-span style="display: inline-block;width: 1px; height: 43px;left:36px;top:1px;z-index:1;background: #BFC2CA;position: absolute;top:0px"></el-span>
                <el-input type="password" style="margin-left: 5px;width: 209px;" v-model="ruleForm2.checkPass" auto-complete="off" placeholder="输入密码"
                  @keyup.enter.native="handleSubmit2"></el-input>
              </div>

            </el-form-item>
            <!-- <el-checkbox v-model="checked" checked class="remember">记住密码</el-checkbox> -->
            <el-form-item style="width: 244px;">
              <el-button type="primary" style="width: 246px;height: 42px;" @click.native.prevent="handleSubmit2" :loading="logining">登录</el-button>
              <!--<el-button @click.native.prevent="handleReset2">重置</el-button>-->
            </el-form-item>
            <el-form-item style="margin-top: 50px">
              <el-span style="display: block; color: #b0aeba;">温馨提示</el-span>
              <el-span style="display: block; color: #b0aeba;">如果你忘记密码与账号请联系我们的</el-span>
              <el-span style="display: block; color: #b0aeba;">客服电话</el-span>
              <el-span style="display: block; color: #b0aeba;">400&nbsp;820&nbsp;4680</el-span>

            </el-form-item>
          </div>


        </el-form>
      </div>
      <div class="Carousel">
        <div class="block">
          <el-carousel height="706px">
            <el-carousel-item v-for="item in carouselimg" :key="item">
              <img v-bind:src="item.url" alt="">
            </el-carousel-item>
          </el-carousel>
        </div>
      </div>
      
    </div>
    <div style="width:100%;position:absolute;bottom:0px;z-index:9999;color:#000000;">
      <a style="text-align:center;font-size:10px;cursor:pointer;text-decoration:none;display:block;color:#000000;" href="http://www.miitbeian.gov.cn">沪ICP备10041964号-1</a>
      <div style="text-align:center;font-size:10px;">Copyright 2010 Jie Yi GPS Co.,Ltd. All rights Reserved 沪ICP备10041964号版权所有，未经许可不得转载</div>
    </div>
  </div>
</template>

<script>
  import {
    requestLogin
  } from "../api/api";
  export default {
    data() {
      return {
        logining: false,
        ruleForm2: {
          account: "",
          checkPass: ""
        },
        carouselimg: [{
            url: require("../assets/banner1.png")
          },
          {
            url: require("../assets/banner2.png")
          }
        ],
        rules2: {
          account: [{
              required: true,
              message: "请输入账号",
              trigger: "blur"
            }
            //{ validator: validaePass }
          ],
          checkPass: [{
              required: true,
              message: "请输入密码",
              trigger: "blur"
            }
            //{ validator: validaePass2 }
          ]
        },
        checked: true
      };
    },
    created () {
      this.baidur()
    },
    
    methods: {
      // 跳新网页
      // baidur(){
      //   window.location.href = "http://www.dangdanggps.com";
      // },
      handleReset2() {
        this.$refs.ruleForm2.resetFields();
      },
      getTo() {
        // 保存用户名
        localStorage.setItem("ruleForm2", JSON.stringify(this.ruleForm2));
        // alert("登录成功");
      },
      handleSubmit2(ev) {
        var _this = this;
        this.$refs.ruleForm2.validate(valid => {
          if (valid) {
            //_this.$router.replace('/table');
            this.logining = true;
            //NProgress.start();
            //          password: window.md5(this.ruleForm2.checkPass)
            var loginParams = {
              username: this.ruleForm2.account,
              password: window.md5(this.ruleForm2.checkPass)
            };
            requestLogin(loginParams).then(data => {
              this.logining = false;
              //NProgress.done();
              let {
                msg,
                code,
                user
              } = data;
              if (code == -1) {
                this.$message({
                  message: "用户名或密码错误",
                  type: "error"
                });
              } else {
                console.log(data.msg)
                let info=JSON.stringify(data.msg)
                sessionStorage.setItem("info", info);
                this.getTo();
                this.$router.push({
                  path: "/echarts", 
                  name: '首页'
                }
                  );
                //        location.href='https://www.baidu.com'; 跳外链
              }
            });
          } else {
            console.log("登录失败");
            return false;
          }
        });
      }
    }
  };
</script>
<style>
.el-div input:-webkit-autofill{
-webkit-box-shadow: 0 0 0 1000px white inset;
}
  .el-carousel__indicators{
    bottom: 55px;
    left: 18.5%;
  }
  .is-active .el-carousel__button{
    background: #2E86DF;
  }
  .el-div {
    overflow: auto;
    border: 1px solid #BFCBD9;
    width: 245px;
    border-radius: 5px;
  }

  .el-div .el-input__inner {
    border: none;
  }
</style>
<style lang="scss" scoped>
  .el_sapn {
    display: block;
    width: 2px;
    height: 44px;
  }

  .demo-ruleForm {
    overflow: hidden;
  }

  .el-input__inner {
    border: none;
  }

  html,
  body {
    margin: 0;
    padding: 0;
    width: 100%;
    height: 100%;

  }

  #app_login {
    width: 100%;
    height: 100%;
    position: relative;
    overflow: hidden;
    background: #f5f7fa;

  }

  .content-bg {
    box-shadow: 0 6px 60px #d6d7db;
    border-radius: 5px;
    overflow: hidden;
    position: absolute;
    left: 50%;
    top: 50%;
    margin-left: -596px;
    margin-top: -330px;
    width: 1194px;
    height: 680px;
    box-sizing: border-box;
    display: flex;
    flex-direction: row;
    background-color: #fff;
    z-index: 6;
  }

  .content {
    z-index: 9;
    border-radius: 5px;
    overflow: hidden;
    position: absolute;
    left: 50%;
    top: 50%;
    margin-left: -605px;
    margin-top: -353px;
    width: 1210px;
    height: 706px;
    box-sizing: border-box;
    display: flex;
    flex-direction: row;
    background-color: #fff;

  }

  .login {
    width: 429px;
    height: 706px;
    box-sizing: border-box;
    position: relative;
  }

  .Carousel {
    width: 781px;
    height: 706px;
    box-sizing: border-box;
  }

  .login-container {
    position: absolute;
    top: 88px;
    left: 50%;
    margin-left: -90px;
  }

  .a1 {
    width: 41px;
    height: 41px;
    border: 1px solid #555;
  }

  .el-carousel__item h3 {
    font-size: 14px;
    opacity: 0.75;
    line-height: 150px;
    margin: 0;
  }

  .login-form {
    margin: 0 auto;
    width: 243px;
    box-sizing: border-box;
  }
</style>