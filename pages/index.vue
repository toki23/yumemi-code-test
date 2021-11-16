<template>
  <div>
    <Header />
    <Prefectures />
    <Graph />
  </div>
</template>

<script>
export default {
  async asyncData({ $axios, $config }) {
    // const prefectures = await $axios.get(
    //   'https://opendata.resas-portal.go.jp/api/v1/prefectures',
    //   {
    //     headers: { 'X-API-KEY': $config.apiKey },
    //   }
    // )
    // console.log(prefectures)
    // return { prefectures }
  },
}
</script>
