<template>
  <div class="register">
    <div class="title">微信用户注册</div>

    <div class="content">
      <van-row>
        <van-col span="2">
          <i class="fa fa-user"></i>
        </van-col>
        <van-col span="22">
          <InputGroup placeholder="请输入账号" v-model="registerForm.name"></InputGroup>
        </van-col>
      </van-row>
      <van-row>
        <van-col span="2">
          <i class="fa fa-lock"></i>
        </van-col>
        <van-col span="22">
          <InputGroup placeholder="请输入密码" v-model="registerForm.passWord" type="password"></InputGroup>
        </van-col>
      </van-row>

      <YButton @click="submitRegister">绑定</YButton>
    </div>

    <div
      @click="loginClick"
      style="color: #ffffff;font-size: 18px;text-align: right; margin-right: 20px; margin-top: 20px"
    >已有账号？去登录</div>

    <div
      @click="registerClick"
      style="color: #ffffff;font-size: 18px;text-align: right; margin-right: 20px; margin-top: 20px"
    >普通用户注册</div>
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
        name: "",
        passWord: ""
      },
      rules: {
        name: [{ required: true, message: "请输入账号", trigger: "blur" }],
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
    registerClick() {
      let currentUrl = encodeURIComponent(
        "http://yun.witsoftware.cn/WEIXINVUE/#/cientRegister"
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
        .post(this.url + "/api/OAuth/WeiXinRegister", this.registerForm)
        .then(res => {
          if (res.data.statusCode == 0) {
            let currentUrl = encodeURIComponent(
              "http://yun.witsoftware.cn/WEIXINVUE/#/login"
            );

            window.location.href =
              "https://open.weixin.qq.com/connect/oauth2/authorize?appid=wx1023731917057f82&redirect_uri=" +
              currentUrl +
              "&response_type=code&scope=snsapi_base&state=STATE&connect_redirect=1#wechat_redirect";
          }
          if (res.data.statusCode == 9) {
            alert(res.data.resultMsg);
          }
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
.register {
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

.footer_wrap {
  position: absolute;
  left: 0;
  bottom: 2.1vw;
  text-align: center;
  width: 100%;
  color: #888;
}

.content,
.btn_wrap {
  margin-top: 6vw;
}

.fa {
  color: #ffffff;
  font-size: 6vw;
}

.van-col--2 {
  margin-top: 14%;
}

.van-col--22 {
  margin-top: 10%;
}

.van-row {
  width: 90%;
  padding-left: 5%;
}

input {
  background: none;
  border: none;
}

.bottom {
  color: #fff;
}

.bottom img {
  width: 6vw;
  height: 6vw;
}

.van-cell {
  background: none;
}

.van-cell__right-icon {
  display: none;
}
</style>

