<script lang="ts" setup>
import useSWRV from 'swrv'
const fetchFactory = (url: string) => fetch(url).then(r => r.json())

const { data } = useSWRV('/api/articles', fetchFactory)

</script>
<template>
  <p>articles length: {{ data === undefined ? 0 : data.length }}</p>
</template>
