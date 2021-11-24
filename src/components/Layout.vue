<script setup>
import CommonHeader from "./CommonHeader.vue";
import {ref} from 'vue'

const isIframe = ref(false)
const myIrfameUrl = ref(null)
axios.get('/navigation/base/page/0').then(res => {
  isIframe.value = !!(res.data.data)
  myIrfameUrl.value = res.data.data
})
</script>

<template>
  <div v-if="isIframe" class="" style="height: 100vh;">
    <iframe
      v-resize="{ log: true }"
      width="100%"
      height="100%"
      :src="myIrfameUrl || iframeUrl"
      frameborder="0"
    ></iframe>
  </div>
  <router-view v-else v-slot="{ Component }">
    <transition>
      <keep-alive>
        <component
          :is="Component"
          class="main-content"
          :key="$route.fullPath"
        />
      </keep-alive>
    </transition>
  </router-view>
</template>

<style scoped>
a {
  color: #42b983;
}

.main-content {
  min-height: 500px;
}
</style>
