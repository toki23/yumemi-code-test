<template>
  <div>
    <p>グラフ</p>
    <div class="linechart-container">
      <LineChart
        v-if="datacollection"
        :chart-data="datacollection"
        :styles="chartStyles"
      />
    </div>
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
      width: window.innerWidth * 0.9,
    }
  },

  computed: {
    chartStyles() {
      return {
        height: `${this.height}px`,
        width: `${this.width}px`,
        position: 'relative',
      }
    },
  },

  watch: {
    selectedPrefectures() {
      const prefectures = this.selectedPrefectures
      const populationCompositionAndPrefecturePromise = prefectures.map(
        // 総人口データの取得
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
            // 取得した総人口データからChart.jsで使用するデータの形式に変換。
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
                fill: false,
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
    window.addEventListener('resize', this.handleResize)
    const populationComposition = await this.$axios.get(
      'https://opendata.resas-portal.go.jp/api/v1/population/composition/perYear?cityCode=-&prefCode=1',
      {
        headers: { 'X-API-KEY': this.$config.apiKey },
      }
    )
    const totalPopulation = populationComposition.data.result.data[0].data
    // 事前に年度データの取得
    this.label = totalPopulation.map((i) => {
      return i.year
    })
  },
  methods: {
    handleResize() {
      this.width = window.innerWidth * 0.95
    },
  },
}
</script>

<style scoped>
.linechart-container {
  display: flex;
  justify-content: center;
}
</style>