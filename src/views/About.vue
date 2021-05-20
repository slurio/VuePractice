<template>
  <div class="about">
    <h1>This is an about page</h1>
  </div>
</template>

<script>
// import { API_KEY } from "../../environment";
import axios from 'axios';

export default {
  created() {
    
    function getRandomArt() {
      axios
      .get("https://collectionapi.metmuseum.org/public/collection/v1/objects")
      .then((resp) => {
        console.log(resp.data)
      })
      .catch((error) => {
        console.log(error.response);
      });
    }

      getRandomArt()
  }


  
};
</script>
