<style scoped>
@import "./pick.less";
</style>
<style>
#app {
  background-color: #efeff7;
}

.top .van-field__control {
  text-align: center !important;
}
</style>
<template>
  <div>
    <div class="top">
      <van-cell-group>
        <van-field v-model="value" placeholder="请输入卡号" maxlength="16"/>
      </van-cell-group>
      <van-cell-group>
        <van-field v-model="password" placeholder="请输入提货码"  maxlength="16" />
      </van-cell-group>
      <van-button size="large" @click="goDetails()">确定提货</van-button>
    </div>
    <div class="gy">
      <p>—当前供应—</p>
      <div v-for="(product,index) in list" :key="index" class="list" @click="toProductInfo(product.id)">
        <img :src="product.imgUrl" alt="" class="img">
      </div>
    </div>
  </div>
</template>
<script>
import { Toast } from "vant";
import service from "./service/index.js";
export default {
  name: "pick",
  mixins: [service],
  data() {
    return {
      value: "",
      password: "",
      // 当前供应
      list: []
    };
  },
  mounted() {
    document.title = "礼卡提货";
    this.selectProByType().then(res => {
      this.list = res.data;
    });
  },
  methods: {
    // 获取cook
    getCookie(name) {
      var arr,
        reg = new RegExp("(^| )" + name + "=([^;]*)(;|$)");
      if ((arr = document.cookie.match(reg))) {
        return unescape(arr[2]);
      } else {
        return null;
      }
    },
    goDetails: function() {
      if (this.value == "") {
        Toast("请输入卡号");
      } else if (this.password == "") {
        Toast("请输入提货码");
      } else {
        this.getLadingDetail({
          staffId: this.getCookie("staffId"),
          token: this.getCookie("token"),
          card: this.value,
          hidecard: this.password
        }).then(res => {
          if (res.code == 100003) {
            Toast("未查询到卡券");
          } else {
            this.$router.push(`/delivery/${this.value}/${this.password}`);
          }
        });
      }
    },
    toProductInfo(productId) {
      this.$router.push(`/product/${productId}`);
    }
  }
};
</script>
