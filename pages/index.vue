<template>
  <div>
    <Header />
    <Prefectures :prefectures="prefectures" />
    <Graph :selectedPrefectures="prefectures.filter((i) => i.selected)" />
  </div>
</template>

<script>
export default {
  async asyncData({ $axios, $config }) {
    const prefectures = await $axios.get(
      'https://opendata.resas-portal.go.jp/api/v1/prefectures',
      {
        headers: { 'X-API-KEY': $config.apiKey },
      }
    )
    return {
      prefectures: prefectures.data.result.map((item) => {
        return { ...item, selected: false }
      }),
    }
  },
}
</script>
<style >
body {
  margin: 0;
}
</style>