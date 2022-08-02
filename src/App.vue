<script setup lang="ts">
// https://github.com/vueuse/head
// you can use this to manipulate the document head in any components,

import type { Ref } from 'vue'

// they will be rendered correctly in the html results with vite-ssg
useHead({
  title: 'Vitesse',
  meta: [
    { name: 'description', content: 'Opinionated Vite Starter Template' },
    {
      name: 'theme-color',
      content: computed(() => isDark.value ? '#00aba9' : '#ffffff'),
    },
  ],
  link: [
    {
      rel: 'icon',
      type: 'image/svg+xml',
      href: computed(() => preferredDark.value ? '/favicon-dark.svg' : '/favicon.svg'),
    },
  ],
})

// 从 hooks 工具函数返回的 ref 变量
const refDataA = ref(1)
const refDataB = ref(2)
setInterval(() => { refDataA.value++; refDataB.value *= 2 }, 1000)

// 在业务中使用
const $refDataA = $(refDataA)
const $refDataB = $(refDataB)

// 类似hooks的use工具函数
function useAdd(...params: Ref<number>[]) {
  return computed(() => params.reduce((acc, param) => { return acc + param.value }, 0))
}

// 需要使用 xxx as unknown as number 断言
// 否则将被视为 JQuery<Ref<number>> 类型
const afterComputed = useAdd($$($refDataA as unknown as number), $$($refDataB as unknown as number))

// 持续输出
watch(afterComputed, () => console.log(afterComputed.value))
</script>

<template>
  <RouterView />
</template>
