<template>
  <div class="app-container">
    <!-- <h1>App 根组件</h1> -->
    <!-- 头部 -->
    <Header title="购物车案例"></Header>
    <!-- 商品列表 -->
    <Goods v-for="item in goods" :key="item.id" :id="item.id" :title="item.goods_name" :pic="item.goods_img" :price="item.goods_price" :state="item.goods_state" :count="item.goods_count" @state-change="getGoodsState"></Goods>
    <!-- 底部结算 -->
    <Footer :isFull="stateFull" :total="priceTotal" :count="countTotal" @state-full-change="getFullState"></Footer>
  </div>
</template>

<script>
import bus from '@/EventBus'
import Header from '@/components/Header/Header.vue'
// import axios from 'axios'
import mock from '@/assets/mock/mock.json'
import Goods from '@/components/Goods/Goods.vue'
import Footer from '@/components/Footer/Footer.vue'
export default {
  name: 'App',
  components: {
    Header,
    Goods,
    Footer
  },
  data() {
    return {
      goods: []
    }
  },
  methods: {
    // https://www.escook.cn/api/cart  get  无参  无法请求
    getGoodsState(e) {
      this.goods.some((item, index) => {
        if (item.id === e.id) {
          item.goods_state = e.state
          return true
        }
      })
      console.log(this.stateFull)
    },
    // 接收全选框状态值
    getFullState(e) {
      this.goods.forEach(item => (item.goods_state = e))
    },
    // 接收到 Counter 组件修改的商品数量后，对商品数据修改
    updateGoods(id, count) {
      this.goods.some(item => {
        if (item.id === id) {
          item.goods_count = count
          return true
        }
      })
    }
  },
  computed: {
    // 统计购物车商品选中状态
    stateFull() {
      return this.goods.every((item, index) => item.goods_state === true)
    },
    // 已选商品 总价钱
    priceTotal() {
      let total = 0
      return this.goods
        .filter(item => item.goods_state)
        .reduce((total, item) => {
          return (total += item.goods_price * item.goods_count)
        }, 0)
    },
    // 结算
    countTotal() {
      return this.goods.filter(item => item.goods_state).reduce((total, item) => (total += item.goods_count), 0)
    }
  },
  created() {
    // 初始化商品列表
    this.goods = mock.list

    // 接收 Counter 组件传递的 商品数量 值
    // 添加时的商品
    bus.$on('count-add', e => {
      this.updateGoods(e.id, e.count)
    })
    // 减少时的商品
    bus.$on('count-sub', e => {
      this.updateGoods(e.id, e.count)
    })
  }
}
</script>

<style lang="less" scoped>
.app-container {
  padding-top: 45px;
  padding-bottom: 50px;
}
</style>
