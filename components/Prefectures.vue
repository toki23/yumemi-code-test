<template>
  <div>
    <p>都道府県</p>
    <div class="checkbox-container">
      <span
        class="checkbox"
        v-for="prefecture in prefectures"
        :key="prefecture.prefCode"
      >
        <input
          :id="prefecture.prefCode"
          v-model="prefecture.selected"
          type="checkbox"
          :name="prefecture.prefCode"
        />
        <label :for="prefecture.prefCode">{{ prefecture.prefName }}</label>
      </span>
    </div>
  </div>
</template>
<script>
export default {
  props: {
    prefectures: {
      type: Array,
      default: null,
    },
  },
}
</script>

<style scoped>
.checkbox-container {
  display: flex;
  flex-direction: row;
  flex-wrap: wrap;
}
.checkbox {
  width: 90px;
}
</style>