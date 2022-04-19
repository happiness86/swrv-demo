<script lang="ts" setup>
import useSWRV from 'swrv'
import Count from '~/components/Count.vue'
const fetchFactory = (url: string) => fetch(url).then(r => r.json())

interface Article {
  id: number
  author: string
  link: string
  title: string
}
const { data: articleList, error, mutate: mutateArticleList } = useSWRV<Article[]>('/api/articles', fetchFactory)
const { data: hideList, mutate: mutateList } = useSWRV<number[]>('/hideArticles', () => [])

const visibleArticles = computed(() => {
  if (articleList.value === undefined)
    return articleList.value

  else
    return articleList.value!.filter(article => !hideList.value?.includes(article.id))
})

const showCount = ref(false)
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
const hide = (id: number) => {
  mutateList(() => hideList.value!.concat([id]))
}

</script>
<template>
  <div v-if="error">
    something got wrong
  </div>
  <ul>
    <li v-for="item in visibleArticles" :key="item.id">
      <span>{{ item.id }}: <a :href="item.link">{{ item.title }}</a></span>
      <button @click="hide(item.id)">
        hide
      </button>
    </li>
  </ul>
  <Count v-if="showCount" />
  <button block border-1 p-2 m-auto @click="updateArticleList">
    update article list
  </button>
</template>
