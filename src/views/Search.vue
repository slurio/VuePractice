<template>
  <div class="search">
    <h1>This is the search page</h1>
    <form>
      <input
        class="searchBar"
        type="text"
        placeholder="Type in here :)"
        v-model="searchTerm"
      />
      <br />
      <select v-model="departmentSelected">
        <option disabled>Please select one</option>
        <option v-for="(department, index) in departments" :key="index">
          {{ department["displayName"] }}
        </option> 
      </select>
      <span>Department</span>
      <label>Medium</label>
      <input type="text" v-model="medium" />
      <br />
      <label>Artist</label>
      <input type="text" v-model="artist" />
      <br />
      <label>Location</label>
      <input type="text" v-model="location" />
      <br />
      <div class="checkBoxFields">
        <input type="checkbox" v-model="permanentCollection" />
        <label>Part of the permanent collection</label>
        <input type="checkbox" v-model="currentlyOnView" />
        <label>Currently on view</label>
        <input type="checkbox" v-model="hasImages" />
        <label>Must have images</label>
      </div>
    </form>
  </div>
</template>

<script>
// import { API_KEY } from "../../environment";
import axios from "axios";

// Create a Form With a Search feature that gets the data
// Create a Discovery Page that has all departments so once user clicks on department it directs them to a new page

export default {
  data() {
    return {
      searchTerm: "",
      departmentSelected: "",
      permanentCollection: false,
      currentlyOnView: false,
      hasImages: false,
      medium: "",
      location: "",
      artist: "",
      departments: [],
    };
  },
  created() {
    axios
      .get(
        "https://collectionapi.metmuseum.org/public/collection/v1/departments"
      )
      .then((resp) => {
        this.departments = resp.data.departments;
      })
      .catch((error) => {
        console.log(error.response);
      });
  },
  
};
</script>

<style scoped>
.search {
  display: flex;
  justify-content: center;
  flex-direction: column;
  border: solid green 1px;
}

.searchBar {
  width: 30%;
  height: 25px;
}

.checkBoxFields {
  display: flex;
  border: red 1px solid;
  justify-content: center;
  margin-top: 25px;
}

.checkBoxFields > input {
  margin-left: 50px;
}
</style>
