<template>
  <div class="login">
    <YButton @click="registerClick" class="register">还没有账号？请先去绑定</YButton>
  </div>
</template>
<script>
let self;
import axios from "axios";
import InputGroup from "../../components/InputGroup";
import YButton from "../../components/YButton";

export default {
  name: "Login",
  components: {
    InputGroup,
    YButton
  },
  data() {
    return {};
  },

  methods: {
    registerClick() {
      let currentUrl = encodeURIComponent(
        "http://yun.witsoftware.cn/WEIXINVUE/#/register"
      );

      window.location.href =
        "https://open.weixin.qq.com/connect/oauth2/authorize?appid=wx1023731917057f82&redirect_uri=" +
        currentUrl +
        "&response_type=code&scope=snsapi_base&state=STATE&connect_redirect=1#wechat_redirect";
    },
    goLogin() {
      let currentUrl = encodeURIComponent(
        "http://yun.witsoftware.cn/WEIXINVUE/#/login"
      );

      window.location.href =
        "https://open.weixin.qq.com/connect/oauth2/authorize?appid=wx1023731917057f82&redirect_uri=" +
        currentUrl +
        "&response_type=code&scope=snsapi_base&state=STATE&connect_redirect=1#wechat_redirect";
    },
    getWxCode() {
      let ua = navigator.userAgent.toLowerCase();

      if (ua.match(/MicroMessenger/i) == "micromessenger") {
        let code = self.GetQueryString("code");
        self.$axios
          .get(self.url + "/api/OAuth/WeiXinLogin?openCode=" + code)
          .then(res => {
            // alert("222222222");
            // alert(res.data.statusCode);
            // alert(res.data.resultMsg);
            // alert("111111111111111");
            if (res.data.statusCode == 0) {
              localStorage.setItem("token", res.data.data);
              self.$router.push("/home");
            }
            if (res.data.statusCode == 9) {
              // console.log(res.data.resultMsg);
              // alert(res.data.statusCode);
              // alert(res.data.resultMsg);
              if (res.data.resultMsg == "用户未注册") {
                alert(res.data.resultMsg);
              } else {
                // setTimeout(() => {
                  // alert("self.goLogin()")
                  self.goLogin();
                // }, 80000);
              }
            }
          })
          .catch(err => {
            // alert("66666666666")
            self.goLogin();
          });
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
    self = this;

    self.getWxCode();
  }
};
</script>
<style scoped>
.login {
  width: 100%;
  height: 100%;
  overflow: hidden;
  padding: 2.1vw;
  box-sizing: border-box;
  background-image: url(../../assets/images/denglu.jpg);
  padding-top: 40%;
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
.register {
  color: #ffffff;
  text-align: right;
  padding-right: 10px;
  padding-top: 20px;
}
</style>

