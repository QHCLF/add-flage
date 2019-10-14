<template>
  <div class="imgCard">
    <div class="box">
      <div class="img-item" v-for="(item, index) in bgPath" :key="index">
        <img :src="item" mode="widthFix" style="width: 100%;" @click="isSelect(item, index)">
        <div class="toggle" :style="{display: index  === curIndex ? 'block' : 'none'}">
          <i class="iconfont icon-check1"></i>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
  export default {
    name: 'imgCard',
    props: {
      bgPath: Array
    },
    data () {
      return {
        curIndex: -1
      }
    },
    mounted () {
      console.log(this.bgPath)
    },
    methods: {
      isSelect (url, index) {
        console.log('选择：' + url + index)
        this.curIndex = index
        this.$emit('url', url)
      }
    }
  }
</script>

<style scoped>
  @import '../../static/style/iconfont.css';
  .imgCard{
    width: 100%;
    height: 125px;
    border-top: gray 1px solid;
    border-bottom: gray 1px solid;
  }
  .box{
    display: flex;
    box-sizing: border-box;
  }
  .img-item{
    box-sizing: border-box;
    margin: 22px;
    flex: 1;
    align-self: center;
  }
  .toggle{
    width: 100px;
    height: 100px;
    border-radius: 3px;
    background-color: rgba(0, 21, 41, 0.5);
    position: absolute;
    top: 12.5px;
    text-align: center;
    line-height: 100px;
  }
  .icon-check1{
    font-size: 30px;
    color: #eeeeee;
  }
</style>
