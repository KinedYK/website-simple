<script setup>
import AdSideC from '@/components/AdSideC.vue'
import SiteListO from '@/components/SiteListO.vue'
import TodayHotNews from '@/components/TodayHotNews.vue'
import SiteRtypesQlist from '@/components/SiteRtypesQlist.vue'
import SiteTypesListN from '@/components/SiteTypesListN.vue'
import RecomNews from '@/components/RecomNews.vue'
import RecomSoft from '@/components/RecomSoft.vue'
import CommonHeader from '@/components/CommonHeader.vue'
import AdRow from "@/components/AdRow.vue";
import AdTime from "@/components/AdTime.vue";
import AdFoot from "@/components/AdFoot.vue";
import AdVideo from '@/components/AdVideo.vue'
import CommonFooter from "@/components/CommonFooter.vue";
import SiteNameList from '@/components/SiteNameList.vue'
import {ref, computed, onMounted, onUnmounted} from 'vue'

const first = ref(null);
const pro1 = axios.get("/navigation/base/first").then((res) => {
  first.value = res.data.data;
  return res
});

const second = ref(null);
const pro2 = axios.get("/navigation/base/second").then((res) => {
  second.value = res.data.data;
  return res
});

const third = ref(null);
const pro3 = axios.get("/navigation/base/third").then((res) => {
  third.value = res.data.data;
  return res
});

const fourth = ref(null);
const pro4 = axios.get("/navigation/base/fourth").then((res) => {
  fourth.value = res.data.data;
  return res
});

let pageNum = ref(1)
const fifth = ref([null]);
const pro5 = axios.get(`/navigation/base/middle/${pageNum.value}.html`).then(res => {
  fifth.value = res.data.data
  return res
})

const sixth = ref(null);
const pro6 = axios.get("/navigation/base/left").then((res) => {
  sixth.value = res.data.data
  return res
})

const loadStatus = ref(false)
const pros = () => {
  Promise.all([pro1, pro2, pro3, pro4, pro5, pro6]).then(ress => {
    console.log(ress, '请求完毕')
    loadStatus.value = true
  })
}

pros();

const scrollTop = ref(0)
const getScrollTop = () => {
  scrollTop.value = document.documentElement.scrollTop
}

onMounted(() => {
  addEventListener('scroll', getScrollTop)
})

onUnmounted(() => {
  removeEventListener('scroll', getScrollTop)
})

// 屏幕宽度
const viewWidth = ref(0)
viewWidth.value = document.documentElement.clientWidth
console.log(viewWidth.value, 'viewWidth')
const adOffsetD = ref(0)
adOffsetD.value = viewWidth.value/2 - 700 > 0 ? viewWidth.value/2 - 700 : 10
const adOffsetB = ref(0)
adOffsetB.value = viewWidth.value/2 - 800 > 0 ? viewWidth.value/2 - 800 : 10

const scrollBottom = (e) => {
  if (pageNum.value == 0) return
  axios.get(`/navigation/base/middle/${++pageNum.value}.html`).then(res => {
    console.log(fifth.value.vos)
    if (res.data.data.vos && res.data.data.vos.length) {
      fifth.value.vos.push(...res.data.data.vos)
    } else {
      pageNum.value = 0
    }
  })
}
</script>

<template>
  <div
    class="index-frame"
    v-if="!loadStatus"
    v-loading="!loadStatus"
    style="height: 100vh"
  ></div>
  <div v-else>
    <!-- 顶部广告 -->
    <AdRow :data="first.a" height="40px" fit="fill"/>

    <div style="position: relative; min-height: 191px; z-index: 999;">
      <CommonHeader :class="{sticky: scrollTop > 40}" :data="second"/>
    </div>

    <div class="flex" style="margin: 7px 0;">
      <SiteNameList :data="second.m"/>
    </div>
    
    <div class="main">
      <div class="sidebar">
        <AdSideC :data="first.c"/>

        <SiteTypesListN :data="third.n"/>

        <div class="m-bt-15">
          <RecomNews :data="sixth"/>
        </div>

        <div class="m-bt-15">
          <RecomSoft :data="third.p"/>
        </div>
      </div>

      <div class="container">
        <SiteListO :data="third.o"/>

        <!-- 今日热点 -->
        <div class="m-bt-15" >
          <TodayHotNews :data="fifth" @to-bottom="scrollBottom"/>
        </div>

        <!-- 网站分类 -->
        <div class="m-bt-15">
          <SiteRtypesQlist :dataR="third.r" :dataQ="third.q"/>
        </div>
      </div>
    </div>

    <!-- 版权信息 -->
    <div class="m-t-15">
      <CommonFooter :data="fourth"/>
    </div>
    
    <!-- 底部广告 -->
    <div class="showCopyRight" style="z-index: 1001;">
      <AdTime :data="first.e" :isBlur="true"/>
    </div>

    <!-- 两边可放大模块 -->
    <div class="ad-d ad-d--l" :style="{top: ((1 * 200)) + 'px', left: adOffsetD + 25 + 'px'}">
      <AdRow v-if="first.d.length > 0" :data="first.d[0]" fit="fill"/>
    </div>
    <div class="ad-d ad-d--l" :style="{top: ((2 * 200)) + 'px', left: adOffsetD + 'px'}">
      <AdRow v-if="first.d.length > 1" :data="first.d[1]" fit="fill"/>
    </div>
    <div class="ad-d ad-d--l" :style="{top: ((3 * 200)) + 'px', left: adOffsetD + 25 + 'px'}">
      <AdRow v-if="first.d.length > 2" :data="first.d[2]" fit="fill"/>
    </div>
    <div class="ad-d ad-d--r" :style="{top: 100 + ((1 * 200)) + 'px', right: adOffsetD + 'px'}">
      <AdRow v-if="first.d.length > 3" :data="first.d[3]" fit="fill"/>
    </div>
    <div class="ad-d ad-d--r" :style="{top: 100 + ((2 * 200)) + 'px', right: adOffsetD + 25 + 'px'}">
      <AdRow v-if="first.d.length > 4" :data="first.d[4]" fit="fill"/>
    </div>

    <!-- 两边的广告B -->
    <div class="ad-b ad-b--l" :style="{left: adOffsetB + 'px'}">
      <AdTime :data="first.b[0]" fit="contain"/>
    </div>
    <div class="ad-b ad-b--r" :style="{right: adOffsetB + 'px'}">
      <AdTime :data="first.b[1]" fit="contain"/>
    </div>

    <!-- 视频广告W -->
    <div v-if="first.w.type == 1" class="ad-w">
      <AdTime :data="first.w" fit="contain"/>
    </div>
    <div v-else class="ad-w">
      <AdVideo :data="first.w"/>
    </div>

    <!-- 开屏广告F -->
    <AdFoot :data="first.f"/>
  </div>
  
</template>

<style lang="less" scoped>
.main {
  width: 1180px;
  margin: auto;
  display: flex;
  justify-content: space-between;
  .sidebar {
    flex-basis: 282px;
    flex-shrink: 0;
    overflow: hidden;
  }
  .container {
    flex-basis: 887px;
    flex-shrink: 0;
    flex-grow: 0;
    margin: 0;
  }
}

.sticky {
  position: fixed !important;
  top: 0;
  left: 0;
  right: 0;
  z-index: 99999;
  background-color: #fff;
}

.ad-b {
  position: fixed;
  max-width: 200px;
  z-index: 1001;
  &--l {
   top: 200px;
   left: 10px;
  }
  &--r {
    top: 200px;
    right: 10px;
  }
}

.ad-d {
  position: fixed;
  max-width: 100px;
  max-height: 100px;
  z-index: 1000;
  &:hover {
    transform: scale(1.1);
  }
  &--l {
   top: 100px;
   left: 10px;
  }
  &--r {
    top: 100px;
    right: 10px;
  }
}

.ad-w {
  position: fixed;
  max-width: 200px;
  max-height: 200px;
  z-index: 989;
  bottom: 0px;
  right: 0px;
}

.showCopyRight {
  position: fixed;
  bottom: 0;
  left: 0;
  right: 0;
  z-index: 995;
}
</style>
