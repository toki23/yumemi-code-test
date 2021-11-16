<template>
  <div>
    <p>グラフ</p>
    <Chart
      v-if="label && dataset"
      :chart-data="datacollection"
      :styles="chartStyles"
    />
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
      dataset: null,
      label: null,
      datacollection: null,
      height: 500,
    }
  },
  watch: {
    selectedPrefectures() {
      const selectedPrefectures = this.selectedPrefectures
      const prefecturesPromise = selectedPrefectures.map(async (prefecture) => {
        return {
          populationComposition: await this.$axios.get(
            `https://opendata.resas-portal.go.jp/api/v1/population/composition/perYear?cityCode=-&prefCode=${prefecture.prefCode}`,
            {
              headers: { 'X-API-KEY': this.$config.apiKey },
            }
          ),
          prefecture,
        }
      })

      Promise.all(prefecturesPromise).then((populationCompositions) => {
        const detaset = populationCompositions.map((populationComposition) => {
          const totalPopulation =
            populationComposition.populationComposition.data.result.data[0].data
          const values = totalPopulation.map((i) => {
            return i.value
          })
          const year = totalPopulation.map((i) => {
            return i.year
          })

          this.label = year

          return {
            label: populationComposition.prefecture.prefName,
            data: values,
          }
        })
        this.dataset = detaset
        this.datacollection = {
          labels: this.label,
          datasets: this.dataset,
        }
      })
    },
  },
  computed: {
    chartStyles() {
      return {
        height: `${this.height}px`,
        position: 'relative',
      }
    },
  },
}
// const totalPopulations = populationCompositions.map(
//   (populationComposition) => {
//     console.log(populationComposition.data.result.data[0])
//     return populationComposition.data.result.data[0].data
//   }
// )
</script>
