<template>
  <div>
    <p>グラフ</p>
    <LineChart :chart-data="datacollection" :styles="chartStyles" />
  </div>
</template>
<script>
export default {
  props: {
    selectedPrefectures: {
      type: Array,
      default: null,
    },
  },
  data() {
    return {
      label: null,
      datacollection: null,
      height: 500,
    }
  },

  computed: {
    chartStyles() {
      return {
        height: `${this.height}px`,
        position: 'relative',
      }
    },
  },
  watch: {
    selectedPrefectures() {
      const prefectures = this.selectedPrefectures
      const populationCompositionAndPrefecturePromise = prefectures.map(
        async (prefecture) => {
          return {
            populationComposition: await this.$axios.get(
              `https://opendata.resas-portal.go.jp/api/v1/population/composition/perYear?cityCode=-&prefCode=${prefecture.prefCode}`,
              {
                headers: { 'X-API-KEY': this.$config.apiKey },
              }
            ),
            prefecture,
          }
        }
      )

      Promise.all(populationCompositionAndPrefecturePromise).then(
        (populationCompositionAndPrefectures) => {
          const dataset = populationCompositionAndPrefectures.map(
            (populationCompositionAndPrefecture) => {
              const totalPopulation =
                populationCompositionAndPrefecture.populationComposition.data
                  .result.data[0].data
              const populationValue = totalPopulation.map((i) => {
                return i.value
              })

              return {
                label: populationCompositionAndPrefecture.prefecture.prefName,
                data: populationValue,
              }
            }
          )
          this.datacollection = {
            labels: this.label,
            datasets: dataset,
          }
        }
      )
    },
  },
  async mounted() {
    const populationComposition = await this.$axios.get(
      'https://opendata.resas-portal.go.jp/api/v1/population/composition/perYear?cityCode=-&prefCode=1',
      {
        headers: { 'X-API-KEY': this.$config.apiKey },
      }
    )
    const totalPopulation = populationComposition.data.result.data[0].data
    this.label = totalPopulation.map((i) => {
      return i.year
    })
  },
}
</script>
