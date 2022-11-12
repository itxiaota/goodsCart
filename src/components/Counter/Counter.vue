<template>
  <div class="number-container d-flex justify-content-center align-items-center">
    <!-- 减 1 的按钮 -->
    <button type="button" class="btn btn-light btn-sm" @click="sub">-</button>
    <!-- 购买的数量 -->
    <span class="number-box">{{ gCount }}</span>
    <!-- 加 1 的按钮 -->
    <button type="button" class="btn btn-light btn-sm" @click="add">+</button>
  </div>
</template>

<script>
import bus from '@/EventBus'
export default {
  props: {
    id: {
      type: Number,
      required: true
    },
    goodsCount: {
      default: 1,
      type: Number
    }
  },
  data() {
    return {
      gCount: this.goodsCount
    }
  },
  methods: {
    add() {
      this.gCount++
      bus.$emit('count-add', { id: this.id, count: this.gCount })
    },
    sub() {
      if (this.gCount === 1) return
      this.gCount--
      bus.$emit('count-sub', { id: this.id, count: this.gCount })
    }
  }
}
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
