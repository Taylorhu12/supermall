<template>
  <div class="bottom-bar">
    <div class="check-button">
      <check-button
        :is-checked="isSelectAll"
        class="check-content"
      ></check-button>
    </div>
    <span>全选</span>
    <div class="price">合计：{{ totalprice }}</div>

    <div class="calculate">去计算({{ checkLength }})</div>
  </div>
</template>

<script>
import CheckButton from "components/content/checkButton/CheckButton";

import { mapGetters } from "vuex";

export default {
  name: "CartBottomBar",
  components: {
    CheckButton,
  },
  computed: {
    ...mapGetters(["cartList"]),
    totalprice() {
      return (
        "￥" +
        this.$store.state.cartList
          .filter((item) => {
            return item.checked;
          })
          .reduce((preValue, item) => {
            return preValue + item.price * item.count;
          }, 0)
          .toFixed(2) //保留两位小数
      );
    },
    checkLength() {
      return this.cartList.filter((item) => item.checked).length;
    },
    isSelectAll() {
      if (this.cartList.length === 0) return false;

      //1.使用filter
      // return !this.cartList.filter((item) => !item.checked).length;

      //2.使用find
      // return !this.cartList.find((item) => !item.checked);

      //3.普通遍历
      for (let item of this.cartList) {
        if (!item.checked) {
          return false;
        }
      }
      return true;
    },
  },
};
</script>

<style scoped>
.bottom-bar {
  display: flex;
  position: relative;
  height: 40px;
  line-height: 40px;
  background-color: #eee;
  font-size: 14px;
}

.check-content {
  display: flex;
  align-items: center;
  width: 40px;
  margin-left: 10px;
}

.check-button {
  width: 20px;
  height: 20px;
  line-height: 20px;
  margin-right: 20px;
  margin-top: 5px;
}

.price {
  margin-left: 30px;
  flex: 1;
}
.calculate {
  width: 90px;
  background-color: red;
  text-align: center;
  color: #fff;
}
</style>