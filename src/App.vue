<template>
  <div class="app-container">
    <Header></Header>
    <Goods
      v-for="item in list"
      :key="item.id"
      :id="item.id"
      :title="item.goods_name"
      :pic="item.goods_img"
      :price="item.goods_price"
      :state="item.goods_state"
      @state-change="getNewState"
      :count="item.goods_count"
    ></Goods>
    <Footer
      :isfull="fullState"
      @full-change="getFullState"
      :amount="amt"
      :all="total"
    ></Footer>
    <p>{{ amt }}</p>
  </div>
</template>

<script>
import axios from "axios";
import Header from "./components/Header/Header.vue";
import Goods from "./components/Goods/Goods";
import Footer from "./components/Footer/Footer.vue";

import bus from "./components/eventBus";
export default {
  data() {
    return {
      list: [],
    };
  },
  computed: {
    //动态计算全选状态
    fullState() {
      return this.list.every((item) => item.goods_state);
    },
    //计算总价格
    amt() {
      return this.list
        .filter((item) => item.goods_state)
        .reduce((total, item) => {
          return (total += item.goods_price * item.goods_count);
        }, 0);
    },
    //已勾选的商品总数
    total() {
      return this.list
        .filter((item) => item.goods_state)
        .reduce((t, item) => {
          return (t += item.goods_count);
        }, 0);
    },
  },
  created() {
    this.initCartList();
    //接收从Counter组件传过来的的值
    bus.$on("share", (val) => {
      this.list.some((item) => {
        if (item.id === val.id) {
          item.goods_count = val.value;
          return true;
        }
      });
    });
  },
  components: {
    Header,
    Goods,
    Footer,
  },
  methods: {
    //请求列表数据
    async initCartList() {
      const { data: res } = await axios.get("https://www.escook.cn/api/cart");
      if (res.status === 200) {
        this.list = res.list;
      }
    },
    //接收子组件传递过来的数据
    getNewState(e) {
      this.list.some((item) => {
        if (item.id === e.id) {
          item.goods_state = e.value;
          return true;
        }
      });
    },
    //接收子组件传递过来的全选按钮的状态
    getFullState(val) {
      this.list.forEach((item) => {
        item.goods_state = val;
      });
    },
  },
};
</script>

<style lang="less" scoped>
.app-container {
  padding-top: 45px;
  padding-bottom: 50px;
}
</style>
