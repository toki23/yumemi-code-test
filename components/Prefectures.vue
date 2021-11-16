<template>
  <div>
    <p>都道府県</p>

    <span v-for="prefecture in prefectures" :key="prefecture.prefCode">
      <input
        :id="prefecture.prefCode"
        v-model="prefecture.selected"
        type="checkbox"
        :name="prefecture.prefCode"
      />
      <label :for="prefecture.prefCode">{{ prefecture.prefName }}</label>
    </span>
  </div>
</template>
<script>
export default {
  props: {
    prefectures: {
      type: Object,
      default: null,
    },
  },
}
</script>
