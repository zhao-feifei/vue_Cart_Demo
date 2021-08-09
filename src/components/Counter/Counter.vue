<template>
  <div
    class="number-container d-flex justify-content-center align-items-center"
  >
    <!-- 减 1 的按钮 -->
    <button type="button" class="btn btn-light btn-sm" @click="sub">-</button>
    <!-- 购买的数量 -->
    <span class="number-box">{{ num }}</span>
    <!-- 加 1 的按钮 -->
    <button type="button" class="btn btn-light btn-sm" @click="add">+</button>
  </div>
</template>

<script>
import bus from "@/components/eventBus/";
export default {
  props: {
    //接收到的数量
    num: {
      type: Number,
      default: 1,
    },
    //接收商品的id，后面使用EventBus传递数量时也需要传id
    id: {
      type: Number,
      required: true,
    },
  },
  methods: {
    add() {
      //注意这里并没有改变num的值
      const obj = { id: this.id, value: this.num + 1 };
      bus.$emit("share", obj);
    },
    sub() {
      if (this.num - 1 === 0) return;
      //注意这里并没有改变num的值
      const obj = { id: this.id, value: this.num - 1 };
      bus.$emit("share", obj);
    },
  },
};
</script>

<style lang="less" scoped>
.number-box {
  min-width: 30px;
  text-align: center;
  margin: 0 5px;
  font-size: 12px;
}

.btn-sm {
  width: 30px;
}
</style>
