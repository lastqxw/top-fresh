<style lang="less" scoped>
@import "./index-event.less";
</style>
<template>
  <div class="event-index">
    <van-row class="e-row" v-if="!this.$route.query.from">
      <van-col class="event-list" span="8" @click.native="goGoodListVC('礼卡',0)"><img src="../../../../assets/icon/lipin.png" width="50" height="50"> 礼卡区</van-col>
      <van-col class="event-list" span="8" @click.native="goGoodListVC('现货',1)"><img src="../../../../assets/icon/xianhuo.png" width="50" height="50">现货区</van-col>
      <van-col class="event-list" span="8" @click.native="goPickupVC()"><img src="../../../../assets/icon/tihuo.png" width="50" height="50">礼卡提货</van-col>
    </van-row>
    <van-row class="e-row" v-else>
      <van-col class="event-list" span="6" @click.native="goGoodListVC('礼卡',0)"><img src="../../../../assets/icon/lipin.png" width="50" height="50"> 礼卡区</van-col>
      <van-col class="event-list" span="6" @click.native="goGoodListVC('现货',1)"><img src="../../../../assets/icon/xianhuo.png" width="50" height="50">现货区</van-col>
      <van-col class="event-list" span="6" @click.native="goPickupVC()"><img src="../../../../assets/icon/tihuo.png" width="50" height="50">礼卡提货</van-col>
      <van-col class="event-list" span="6" @click.native="Traceability()"><img src="../../../../assets/icon/candi.png" width="50" height="50">产地溯源</van-col>
    </van-row>
  </div>
</template>
<script>
  import { Dialog } from "vant";

  export default {
  data() {
    return {
      staffId: this.getCookie("staffId"),
      token: this.getCookie("token"),
    };
  },
  beforeMount() {},
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
    // 判断用户是否登录
    isToken() {
      var token = this.getCookie("token");
      if (token) {
        return true;
      } else {
        return false;
      }
    },
    Traceability() {
      let from = this.$route.query.from;
      if (from == "IOS") {
        this.$bridge.callHandler(
          "goGoodOriginVC",
          {
            name: name
          },
          data => {
          }
        );
      } else if (from == "Android") {
        this.$bridge.callHandler(
          "goGoodOriginVC",
          {
            name: name
          },
          data => {
          }
        );
      }
    },
    goGoodListVC(name, id) {
      let from = this.$route.query.from;
      if (from == "IOS") {
        this.$bridge.callHandler(
          "goGoodListVC",
          {
            name: name
          },
          data => {
          }
        );
      } else if (from == "Android") {
        this.$bridge.callHandler(
          "goGoodListVC",
          {
            name: name
          },
          data => {
          }
        );
      } else {
        this.$router.push(`/goodsList/${id}`);
      }
    },
    goPickupVC() {
      var istoken = this.isToken();
      if (istoken) {
        let from = this.$route.query.from;
        if (from == "IOS") {
          this.$bridge.callHandler("goPickupVC", data => {
          });
        } else if (from == "Android") {
          this.$bridge.callHandler("goPickupVC", data => {
          });
        } else {
          this.$router.push(`/pick`);
        }
      } else {
        Dialog.confirm({
          title: "提示",
          message: "请先登录您的账户",
          confirmButtonText: "去登录"
        }).then(() => {
          this.$router.push(`/login`);
        }).catch()
      }

    },
    goGoodOriginVC() {
      let from = this.$route.query.from;
      if (from == "IOS") {
        this.$bridge.callHandler("goGoodOriginVC", data => {
        });
      } else if (from == "Android") {
        this.$bridge.callHandler("goGoodOriginVC", data => {
        });
      } else {
        this.$router.push(`/pick`);
      }
    }
  },
  mounted() {},
  computed: {
    viewWeight: function() {
      return (window.innerWidth - 20) / 2 + "px";
    }
  }
};
</script>
