<script>
import { Line } from 'vue-chartjs'

export default {
  extends: Line,
  name: 'chart',
  props: ['Label', 'Data'],
  data() {
    return {
      data: {
        labels: this.Label,
        datasets: [
          {
            label: 'fjldksj',
            data: this.Data,
            borderWidth: 1,
          },
        ],
      },
    }
  },
  mounted() {
    console.log("skdfjs");
    console.log(this.Label)
    console.log(this.Data)
    this.renderChart(this.data, this.options)
  },
}
</script>

<style>
</style>
