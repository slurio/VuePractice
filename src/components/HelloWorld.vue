<template>
  <div class="hello">
    <h1>{{ msg }}</h1>
    <p>
      For more information about the Metropolitan Museum of Art<br />
      check out
      <a href="https://www.metmuseum.org/" target="_blank" rel="noopener"
        >here</a
      >.
    </p>
  </div>
</template>

<script>
export default {
  name: "HelloWorld",
  props: {
    msg: String,
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
a {
  color: black;
}
</style>
