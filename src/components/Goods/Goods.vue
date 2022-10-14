<template>
  <div class="goods-container">
    <!-- 左侧图片 -->
    <div class="thumb">
      <div class="custom-control custom-checkbox">
        <!-- 复选框 -->
        <input
          type="checkbox"
          class="custom-control-input"
          :id="'cd'+ id"
          :checked="state"
          @change="change"
        />
        <label class="custom-control-label" :for="'cd' + id">
          <!-- 商品的缩略图 -->
          <img :src="imgs" alt="" />
        </label>
      </div>
    </div>
    <!-- 右侧信息区域 -->
    <div class="goods-info">
      <!-- 商品标题 -->
      <h6 class="goods-title">{{ title }}</h6>
      <div class="goods-info-bottom">
        <!-- 商品价格 -->
        <span class="goods-price">￥{{ newPrice }}</span>
        <!-- 商品的数量 -->
        <Counter @share_count="getNewCount" :counts="this.count"></Counter>
      </div>
    </div>
  </div>
</template>

<script>
import Counter from "@/components/Counter/Counter.vue";
import Bus from '@/components/EventBus.js'
export default {
  components: {
    Counter,
  },
  props: {
    //ID
    id: {
      require: true,
      type: Number,
    },
    //图片
    img: {
      default: "",
      typeof: String,
    },
    //标题
    title: {
      default: "",
      typeof: String,
    },
    //单价
    price: {
      default: "",
      type: Number,
    },
    //勾选状态
    state: {
      default: "",
      type: Boolean,
    },
    //件数
    count: {
      default: true,
      type: Number,
    },
  },
  data() {
    return {
      // 可以不做存入data 直接上面传 img
      imgs: this.img,
      
      newPrice : this.price
    };
  },
  methods: {
    change(e) {
      //监听单选框的 变化
      //  console.log(e.target.checked);
      let newState = e.target.checked;
      this.$emit("share", { id: this.id, status : newState });
    },
    getNewCount(val){
      // console.log(val);
     this.newPrice =  this.price * val
     //把传递的传给 父组件
     this.$emit('share_newCounts',{id: this.id,val : val})
    //  console.log(val);
   
   
    }
  },
};
</script>

<style lang="less" scoped>
.goods-container {
  + .goods-container {
    border-top: 1px solid #efefef;
  }
  padding: 10px;
  display: flex;
  .thumb {
    display: flex;
    align-items: center;
    img {
      width: 100px;
      height: 100px;
      margin: 0 10px;
    }
  }

  .goods-info {
    display: flex;
    flex-direction: column;
    justify-content: space-between;
    flex: 1;
    .goods-title {
      font-weight: bold;
      font-size: 12px;
    }
    .goods-info-bottom {
      display: flex;
      justify-content: space-between;
      .goods-price {
        font-weight: bold;
        color: red;
        font-size: 13px;
      }
    }
  }
}
</style>
