<template>
  <div>
    <p>都道府県</p>

    <span v-for="prefecture in prefectures" :key="prefecture.prefCode">
      <input
        type="checkbox"
        :id="prefecture.prefCode"
        :name="prefecture.prefCode"
      />
      <label :for="prefecture.prefCode">{{ prefecture.prefName }}</label>
    </span>
  </div>
</template>
<script>
export default {
  props: ['prefectures'],

  mounted() {
    console.log(this.prefectures)
  },
}
</script>
