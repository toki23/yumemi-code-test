<template>
  <div>
    <Header />
    <Prefectures :prefectures="this.prefectures" />
    <Graph />
  </div>
</template>

<script>
export default {
  mounted() {},
  async asyncData({ $axios, $config }) {
    const prefectures = await $axios.get(
      'https://opendata.resas-portal.go.jp/api/v1/prefectures',
      {
        headers: { 'X-API-KEY': $config.apiKey },
      }
    )
    console.log(prefectures.data.result)
    return { prefectures: prefectures.data.result }
  },
}
</script>
