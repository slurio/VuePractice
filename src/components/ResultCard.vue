<template>
  <div>
    <img :src="result.primaryImageSmall" :alt="result.title" />
  </div>
</template>

<script>
export default {
  props: {
    result: Object,
  },
};
</script>

<style scoped>
img {
  width: 500px;
  height: auto;
  margin: 5px;
}
</style>
