<template>
  <div>
    <Header />
    <Prefectures :prefectures="prefectures" />
    <Graph :selected-prefectures="selectedPrefectures" />
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
        return { ...item, selected: false } // チェックボックスで選択されたかを示す変数selectedの追加
      }),
    }
  },
  computed: {
    selectedPrefectures() {
      return this.prefectures.filter((i) => i.selected) ?? []
    },
  },
}
</script>
<style >
body {
  margin: 0;
}
</style>