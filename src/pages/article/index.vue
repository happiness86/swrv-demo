<script lang="ts" setup>
import useSWRV from 'swrv'
import Count from '~/components/Count.vue'
const fetchFactory = (url: string) => fetch(url).then(r => r.json())

const { data, error, mutate: mutateArticleList } = useSWRV('/api/articles', fetchFactory)

const showCount = ref(true)
setTimeout(() => showCount.value = true, 3000)

const updateArticleList = async() => {
  await fetch('/api/articles', {
    method: 'POST',
    headers: { 'Content-Type': 'application/json' },
    body: JSON.stringify({
      title: `Untitled article ${Math.ceil(Math.random() * 100)}`,
      author: 'Andy',
      link: 'https://www.vuemastery.com/blog/part-3-client-side-graphql-with-vuejs',
    }),
  })
  mutateArticleList()
}
</script>
<template>
  <div v-if="error">
    something got wrong
  </div>
  <div v-for="(item, index) in data" :key="item.id">
    {{ index }}: <a :href="item.link">{{ item.title }}</a>
  </div>
  <Count v-if="showCount" />
  <button block border-1 p-2 m-auto @click="updateArticleList">
    update article list
  </button>
</template>
