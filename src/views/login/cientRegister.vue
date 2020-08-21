<template>
  <div class="cient_register">
    <div class="title">普通用户注册</div>

    <div class="content">
      <el-form
        :model="registerForm"
        :rules="rules"
        ref="registerForm"
        label-width="100px"
        class="demo-ruleForm"
      >
        <el-form-item label="公司名称：" prop="companyName">
          <el-input v-model="registerForm.companyName" placeholder="请输入公司名称" />
        </el-form-item>
        <el-form-item label="用户名：" prop="userName">
          <el-input v-model="registerForm.userName" placeholder="请输入用户名" />
        </el-form-item>
        <el-form-item label="手机号：" prop="phoneNo">
          <el-input v-model="registerForm.phoneNo" placeholder="请输入手机号" />
        </el-form-item>
        <el-form-item label="登录名：" prop="name">
          <el-input v-model="registerForm.name" placeholder="请输入登录名" />
        </el-form-item>
        <el-form-item label="密码：" prop="passWord">
          <el-input v-model="registerForm.passWord" placeholder="请输入密码" />
        </el-form-item>

        <el-form-item label="备注：" prop="remark">
          <el-input v-model="registerForm.remark" placeholder="请输入备注"></el-input>
        </el-form-item>
      </el-form>
      <div class="footer">
        <el-button @click="submitRegister()">注册</el-button>
      </div>
    </div>

    <div
      @click="loginClick"
      style="color: #ffffff;font-size: 18px;text-align: right; margin-right: 20px; margin-top: 20px"
    >已有账号？去登录</div>
  </div>
</template>
<script>
import axios from "axios";
import InputGroup from "../../components/InputGroup";
import YButton from "../../components/YButton";

export default {
  name: "register",
  components: {
    InputGroup,
    YButton
  },
  data() {
    return {
      registerForm: {
        openCode: "",
        companyName: "",
        userName: "",
        phoneNo: "",
        name: "",
        passWord: "",
        remark: ""
      },
      rules: {
        companyName: [
          { required: true, message: "请输入公司名称", trigger: "blur" }
        ],
        userName: [
          { required: true, message: "请输入用户名", trigger: "blur" }
        ],
        phoneNo: [{ required: true, message: "请输入手机号", trigger: "blur" }],

        name: [{ required: true, message: "请输入登录名", trigger: "blur" }],
        passWord: [{ required: true, message: "请输入密码", trigger: "blur" }]
      },
      disabled: false
    };
  },
  computed: {},
  methods: {
    loginClick() {
      let currentUrl = encodeURIComponent(
        "http://yun.witsoftware.cn/WEIXINVUE/#/login"
      );

      window.location.href =
        "https://open.weixin.qq.com/connect/oauth2/authorize?appid=wx1023731917057f82&redirect_uri=" +
        currentUrl +
        "&response_type=code&scope=snsapi_base&state=STATE&connect_redirect=1#wechat_redirect";
    },
    submitRegister() {
      let ua = navigator.userAgent.toLowerCase();
      if (ua.match(/MicroMessenger/i) == "micromessenger") {
      }

      this.$axios
        .post(this.url + "/api/OAuth/WeiXinClientRegister", this.registerForm)
        .then(res => {
        //   console.log(res.data);
        //   alert(res.data.statusCode);
          alert(res.data.resultMsg);
          //   if (res.data.statusCode == 0) {
          //     let currentUrl = encodeURIComponent(
          //       "http://yun.witsoftware.cn/WEIXINVUE/#/login"
          //     );

          //     window.location.href =
          //       "https://open.weixin.qq.com/connect/oauth2/authorize?appid=wx1023731917057f82&redirect_uri=" +
          //       currentUrl +
          //       "&response_type=code&scope=snsapi_base&state=STATE&connect_redirect=1#wechat_redirect";
          //   }
          //   if (res.data.statusCode == 9) {
          //     alert(res.data.resultMsg);
          //   }
        })
        .catch(err => {});
    },

    getWxCode() {
      let ua = navigator.userAgent.toLowerCase();

      if (ua.match(/MicroMessenger/i) == "micromessenger") {
        let code = this.GetQueryString("code");

        this.registerForm.openCode = code;
      }
    },
    GetQueryString(name) {
      let url = new RegExp("(^|&)" + name + "=([^&]*)(&|$)");

      let newUrl = window.location.search.substr(1).match(url);

      if (newUrl != null) {
        return unescape(newUrl[2]);
      } else {
        return false;
      }
    }
  },
  mounted() {
    this.getWxCode();
  }
};
</script>
<style scoped>
.cient_register {
  width: 100%;
  height: 100%;
  overflow: hidden;
  /* padding: 2.1vw; */
  box-sizing: border-box;
  background-image: url(../../assets/images/denglu.jpg);
}

.title {
  margin-top: 20vw;
  font-size: 5.1vw;
  text-align: center;
  color: #ffffff;
}

.content {
  margin-top: 6vw;
}

/* input {
  background: none;
  border: none;
} */
</style>
<style>
.cient_register .el-form-item__label {
  color: #ffffff;
  font-size: 14px;
}
.cient_register .el-input {
  display: inline-block;
  /* height: 47px; */
  width: 85%;
  border-bottom: 1px solid #ffffff;
}

.cient_register .el-input input {
  background: transparent;
  color: #ffffff;
  border: 0px;
  -webkit-appearance: none;
  border-radius: 0px;
  caret-color: #ffffff;
}
.cient_register .footer .el-button {
  background: #5db75d !important;
  width: 80%;
  border-color: #5db75d !important;
  margin-left: 10%;
  color: #ffffff;
  font-size: 18px !important;
}
</style>

