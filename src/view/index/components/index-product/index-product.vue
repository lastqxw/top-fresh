<style scoped lang="less">
@import "./index-product.less";
</style>
<style>
.commodityList .van-tab--active {
  color: #fff !important;
  background: #000 !important;
}
.commodityList .van-tabs__nav--card {
  margin: 0 20%;
}
.commodityList .van-tabs__content {
  margin: 10px 10px 0 10px;
}
.r-product .van-tabs__nav--card {
  border: 1px solid #000 !important;
}
.r-product .van-tabs__nav--card .van-tab {
  color: black !important;
}
.r-product .van-tab--active {
}
.r-product .van-tabs__nav--card .van-tab--active {
  color: #fff !important;
}
</style>
<template>
  <div class="r-product">
    <div class="r-title">
      <p>用心推荐</p>
      <span>给您更好的享受</span>
    </div>
    <van-tabs type="card" class="r-tab commodityList" @click="getInfo">
      <van-tab v-for="index in 2" :title="index == 1 ? '礼卡' : '现货'" :key="index">
        <div class="gy" v-if="index == 1">
          <div v-for="(product,index) in imgList" :key="index" class="list">
            <img :src="product.imgUrl" class="img" @click="goGoodInfoVC(product.id)">
          </div>
        </div>
        <div class="gy" v-if="index == 2">
          <div v-for="(product,index) in imgList" :key="index" class="list">
            <img :src="product.imgUrl" class="img" @click="goGoodInfoVC(product.id)">
          </div>
        </div>
      </van-tab>
    </van-tabs>
    <div class="r-more" @click="goGoodListVC()">
      <span>查看全部</span>
      <van-icon name="more" />
    </div>
  </div>
</template>
<script>
import indexService from "../../service/index.js";
export default {
  mixins: [indexService],
  data() {
  return {
      active: 0,
      imgList: "",
      name: '礼卡'
    };
  },
  beforeMount() {
    this.getproducts(1).then(res => {
      this.imgList = res;
    });
  },
  methods: {
    getInfo(index) {
      if (index == 0) {
        this.active = 0;
        this.name = "礼卡";
        this.getproducts(index + 1).then(res => {
          this.imgList = res;
        });
      } else {
        this.name = "现货";
        this.active = 1;
        this.getproducts(index + 1).then(res => {
          this.imgList = res;
        });
      }
    },
    goGoodListVC(name, index) {
      let from = this.$route.query.from;
      if (from == "IOS") {
        this.$bridge.callHandler("goGoodListVC", { name: this.name }, data => {
        });
      } else if (from == "Android") {
        this.$bridge.callHandler("goGoodListVC", { name: this.name }, data => {
        });
      } else {
        this.$router.push(`/goodsList/${this.active}`);
      }
    },
    goGoodInfoVC(productId) {
      let from = this.$route.query.from;
      if (from == "IOS") {
        this.$bridge.callHandler(
          "goGoodInfoVC",
          { productId: productId },
          data => {
          }
        );
      } else if (from == "Android") {
        this.$bridge.callHandler(
          "goGoodInfoVC",
          { productId: productId },
          data => {
          }
        );
      } else {
        this.$router.push(`/product/${productId}`);
      }
    }
  },
  computed: {
    viewWeight: function() {
      return (window.innerWidth - 20) / 2 + "px";
    }
  }
};
</script>
