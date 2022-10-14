<template>
  <div class="number-container d-flex justify-content-center align-items-center">
    <!-- 减 1 的按钮 -->
    <button type="button" class="btn btn-light btn-sm" @click="jian">-</button>
    <!-- 购买的数量 -->
    <span class="number-box">{{ counts }}</span>
    <!-- 加 1 的按钮 -->
    <button type="button" class="btn btn-light btn-sm" @click="jia">+</button>
  </div>
</template>

<script>
export default {
  props:['counts'],
 data () {
  return {
    count : this.counts
  }
 },
  methods:{
 
    jia(){
      this.count = this.count + 1
     this.$emit('share_count',this.count)
    //  console.log(this.counts);

    },
    jian(){
   
      if (this.count >1) {
        this.count = this.count - 1 
      }else{
        this.count = this.count
      }
        this.$emit('share_count',this.count)

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
