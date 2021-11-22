<script setup>
import {ref} from 'vue'
const list = ref(["电视剧", "电影","游戏","页游","新闻","社会","旅游","音乐"])
defineProps({
  data: {
    type: Array,
  }
})
</script>

<template>
  <div class="site m-border-1">
    <el-row>
      <el-col class="flex flex-center-row" v-for="i in ~~(data.length/2)" :key="i" :span="12">
        <span class="text text-l">
          <a @click="gotoHref(data[(i-1)*2].url)">
            {{data[(i-1)*2].name}}
          </a>
        </span>
        <span class="split">|</span>
        <span class="text text-r">
          <a @click="gotoHref(data[(i-1)*2].url)">{{data[2*i-1].name}}</a>
        </span>
      </el-col>
    </el-row>
  </div>
</template>

<style lang="less" scoped>
.site {
  // padding: 10px 10px;
  .text {
    display: inline-block;
    width: 80px;
    font-family: PingFangSC-Regular;
    font-size: 14px;
    color: #333333;
    letter-spacing: 0;
    text-align: center;
    font-weight: 400;
    line-height: 2;
    &-l {
      text-align: right;
    }
    &-r {
      text-align: left;
    }
  }
  .split {
    color: #CFD4DB;
    margin: 0 11px;
  }
}
  
</style>