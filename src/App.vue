<template>
  <div class="app-container">
    <Header></Header>
    <!-- <h1>App 根组件{{ fullState }}</h1> -->
    价钱：{{ amt }}
    商品数量：{{ Sum_count }}
    <Goods
      v-for="item in list"
      :key="item.id"
      :id="item.id"
      :img="item.goods_img"
      :title="item.goods_name"
      :price="item.goods_price"
      :state="item.goods_state"
      :count="item.goods_count"
      @share="getNewStats"
      @share_newCounts="sumCount"
    ></Goods>

    <Footer
      :flag="fullState"
      @fullNewstate="getFullstate"
      :newSumCount="SumCount"
      :SumPrice="amt"
    ></Footer>
  </div>
</template>

<script>
//导入 axios 请求插件
import axios from "axios";
//导入Header 组件
import Header from "@/components/Header/Header.vue";
import Goods from "@/components/Goods/Goods.vue";
import Footer from "@/components/Footer/Footer.vue";

export default {
  //注册组件
  components: {
    Header,
    Goods,
    Footer,
  },
  //属性数据
  data() {
    return {
      //用来存储购物车里面的数据
      list: [],
      //存储SumCount 总件数
      SumCount: 0,
     
    };
  },
  //计算属性
  computed: {
    //动态计算出全选的状态是 true 还是 false
    fullState() {
      return this.list.every((item) => item.goods_state === true);
    },
    //已勾选商品的总价格
    amt(){
      //1.先 filter 过滤
      //2.再reduce 累加
      return this.list
      .filter(item=>item.goods_state)
      .reduce((tolad,item)=>(tolad += item.goods_price * item.goods_count),0 )
    },
    //计算商品件数的计算属性跟老师走的
    Sum_count(){
    return  this.list.filter(item=>item.goods_state)
    .reduce((tolad,item)=>(tolad += item.goods_count),0)
    }
  },
  //方法
  methods: {
    //封装请求列表方法
    async initCarList() {
      //调用 axios 的 get 请求
      const { data: res } = await axios.get("https://www.escook.cn/api/cart");
      // console.log(res.list);
      this.list = res.list;
      //当 组件生成时自动获取 勾选的件数
      let sum = 0;
      res.list.forEach((item) => {
        if (item.goods_state == true) {
          sum = sum + item.goods_count;
        }
      });
      this.SumCount = sum;
    },

    //子组件 goos 传过来的数据
    getNewStats(val) {
      // console.log(val);
      this.list.some((item) => {
        // console.log(item.id);
        if (item.id == val.id) {
          item.goods_state = val.status;
          return true;
        }
      });
      //点击 选择按钮勾选时 触发计算 商品件数
      let sum = 0;
      this.list.forEach((item) => {
        if (item.goods_state === true) {
          sum = sum + item.goods_count;
        }
      });
      // console.log(sum);
      this.SumCount = sum;
    },
    //Footer 传过来的值
    getFullstate(val) {
      // console.log(val);
      // this.list.forEach(item => {
      //   if (val === true) {
      //     item.goods_state = true
      //   }else{
      //     item.goods_state = false
      //   }
      // })
      //简化
      this.list.forEach((item) => (item.goods_state = val));
      //点击全选按钮时的 计算总总数
      let sum = 0
      this.list.forEach((item)=>{
        // console.log(val);
        if (val == true) {
          sum  = sum + item.goods_count
        }
      })
      this.SumCount = sum;
      
    },
    sumCount(val) {
      // console.log(val);
      //当前孙组件Counter组件 端页面改变时 把数据传达数据 list 来改变list
      this.list.some((item) => {
        if (item.id == val.id) {
          item.goods_count = val.val;
        }
      });
      //点击 加减商品时 来计算商品件数
      console.log(val);
      let sum = 0;
      this.list.forEach((item) => {
        if (item.goods_state == true) {
          sum = sum + item.goods_count;
        }
      });
      this.SumCount = sum;
      console.log(sum);
    },
   
  },
  //生命周期的钩子函数
  created() {
    this.initCarList();
  },
};
</script>

<style lang="less" scoped>
.app-container {
  padding-top: 45px;
  padding-bottom: 50px;
}
</style>
