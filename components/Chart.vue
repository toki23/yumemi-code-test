<script>
import { Line, mixins } from 'vue-chartjs'
const { reactiveProp } = mixins
export default {
  extends: Line,
  mixins: [reactiveProp],
  name: 'chart',
  // props: ['label', 'dataset'],
  data() {
    return {
      data: {
        labels: this.label,
        datasets: this.dataset,
      },
    }
  },
  mounted() {
    console.log('skdfjs')
    console.log(this.label)
    console.log(this.dataset)
    this.renderChart(this.chartData, this.options)
  },
  watch: {
    dataset() {},
  },
}
</script>

<style>
</style>
