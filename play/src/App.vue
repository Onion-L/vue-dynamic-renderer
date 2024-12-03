<template>
  <button @click="showRemoteChild = true">加载远程组件</button>
  <RemoteChild v-if="showRemoteChild" />
</template>

<script setup lang="ts">
import { defineAsyncComponent, ref } from 'vue'
import * as Vue from 'vue'
import { loadModule } from 'vue3-sfc-loader'

const showRemoteChild = ref(false)

const options = {
  moduleCache: {
    vue: Vue
  },
  async getFile(url: any) {
    const res = await fetch(url)
    const code = await res.text()
    return code
  },
  addStyle(textContent: any) {
    const style = Object.assign(document.createElement('style'), {
      textContent
    })
    const ref = document.head.getElementsByTagName('style')[0] || null
    document.head.insertBefore(style, ref)
  }
}

const RemoteChild = defineAsyncComponent(async () => {
  const res = await loadModule(
    'http://localhost:8080/remote-component.vue',
    options
  )
  console.log('res', res)
  return res
})
</script>

<style scoped>
.divider {
  background-color: red;
  width: 100vw;
  height: 1px;
  margin: 20px 0;
}
</style>
