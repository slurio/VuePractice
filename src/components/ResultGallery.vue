<template>
  <div>
    <ResultCard v-for="result in results" :key="result.data.objectID" :result="result.data"/>
  </div>
</template>

<script>
import ResultCard from "./ResultCard.vue";

export default {
  components: {
    ResultCard,
  },
  props: {
    results: Array,
  },
};
</script>

<style scoped>
div {
  display: flex;
  flex-wrap: wrap;
}
</style>
