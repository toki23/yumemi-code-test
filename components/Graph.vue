<template>
  <div>
    <p>グラフ</p>
    <Chart v-if="label && data" :Label="label" :Data="data" />
  </div>
</template>
<script>
export default {
  data() {
    return {
      label: null,
      data: null,
    }
  },
  async mounted() {
    const populationComposition = await this.$axios.get(
      'https://opendata.resas-portal.go.jp/api/v1/population/composition/perYear?cityCode=-&prefCode=11',
      {
        headers: { 'X-API-KEY': this.$config.apiKey },
      }
    )
    const totalPopulation = populationComposition.data.result.data[0]
    const chartLabel = totalPopulation.data.map((data) => data.year)
    const chartData = totalPopulation.data.map((data) => data.value)
    this.label = chartLabel
    this.data = chartData
  },
}
</script>
