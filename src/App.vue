<template>
  <div class="app-container">
    <Header></Header>
    <Goods
      v-for="item in list"
      :key="item.id"
      :title="item.goods_name"
      :pic="item.goods_img"
    ></Goods>
  </div>
</template>

<script>
import axios from "axios";
import Header from "./components/Header/Header.vue";
import Goods from "./components/Goods/Goods";
export default {
  data() {
    return {
      list: [],
    };
  },
  created() {
    this.initCartList();
  },
  components: {
    Header,
    Goods,
  },
  methods: {
    //请求列表数据
    async initCartList() {
      const { data: res } = await axios.get("https://www.escook.cn/api/cart");
      if (res.status === 200) {
        this.list = res.list;
      }
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
