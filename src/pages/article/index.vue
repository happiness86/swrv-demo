<script lang="ts" setup>
import useSWRV from 'swrv'
import Count from '~/components/Count.vue'
const fetchFactory = (url: string) => fetch(url).then(r => r.json())

const { data, error } = useSWRV('/api/articles', fetchFactory)

const showCount = ref(false)
setTimeout(() => showCount.value = true, 3000)
</script>
<template>
  <div v-if="error">
    something got wrong
  </div>
  <div v-for="item in data" :key="item.id">
    <a :href="item.link">{{ item.title }}</a>
  </div>
  <Count v-if="showCount" />
</template>
