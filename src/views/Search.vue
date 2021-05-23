<template>
  <div class="search">
    <h1>This is the search page</h1>
    <form @submit.prevent="searchInput">
      <input
        class="searchBar"
        type="text"
        placeholder="Type in here :)"
        v-model="searchObj.searchTerm"
      />
      <br />
      <select v-model="searchObj.departmentSelected">
        <option disabled>Please select one</option>
        <option v-for="(department, index) in departments" :key="index">
          {{ department["displayName"] }}
        </option> 
      </select>
      <span>Department</span>
      <label>Medium</label>
      <input type="text" v-model="searchObj.medium" />
      <br />
      <label>Artist</label>
      <input type="text" v-model="searchObj.artist" />
      <br />
      <label>Location</label>
      <input type="text" v-model="searchObj.location" />
      <br />
      <div class="checkBoxFields">
        <input type="checkbox" v-model="searchObj.permanentCollection" />
        <label>Part of the permanent collection</label>
        <input type="checkbox" v-model="searchObj.currentlyOnView" />
        <label>Currently on view</label>
        <input type="checkbox" v-model="searchObj.hasImages" />
        <label>Must have images</label>
      </div>
      <input type="submit" value="Submit"/>
    </form>
  </div>
</template>

<script>
import axios from "axios";

// Create a Form With a Search feature that gets the data
// Create a Discovery Page that has all departments so once user clicks on department it directs them to a new page

export default {
  data() {
    return {
      departments: [],
      searchObj: this.createFreshObj(),
      searchResults: [],
    };
  },
  methods: {
    createFreshObj() {
      return {
        searchTerm: "",
        departmentSelected: "",
        permanentCollection: false,
        currentlyOnView: false,
        hasImages: false,
        medium: "",
        location: "",
        artist: "",
      }
    },
    searchInput() {
      let searchRequest = "https://collectionapi.metmuseum.org/public/collection/v1/search?";
      if (this.searchObj.departmentSelected) {
        let departmentId = this.departments.find(
          (department) =>
            department.displayName === this.searchObj.departmentSelected
        ).departmentId;
        searchRequest += "departmentId=" + departmentId.toString();
      }
      if (this.searchObj.permanentCollection) {
        searchRequest.charAt(searchRequest.length - 1) === "?"
          ? (searchRequest += "isHighlight=true")
          : (searchRequest += "&isHighlight=true");
      }
      if (this.searchObj.currentlyOnView) {
        searchRequest.charAt(searchRequest.length - 1) === "?"
          ? (searchRequest += "isOnView=true")
          : (searchRequest += "&isOnView=true");
      }
      if (this.searchObj.hasImages) {
        searchRequest.charAt(searchRequest.length - 1) === "?"
          ? (searchRequest += "hasImages=true")
          : (searchRequest += "&hasImages=true");
      }
      if (this.searchObj.medium) {
        searchRequest.charAt(searchRequest.length - 1) === "?"
          ? (searchRequest += "medium=" + this.searchObj.medium)
          : (searchRequest += "&medium=" + this.searchObj.medium);
      }
      if (this.searchObj.location) {
        let capitalLocation = this.searchObj.location.charAt(0).toUpperCase() + this.searchObj.location.slice(1);
        searchRequest.charAt(searchRequest.length - 1) === "?"
          ? (searchRequest += "geoLocation=" + capitalLocation)
          : (searchRequest += "&geoLocation=" + capitalLocation);
      }
      if (this.searchObj.artist) {
        searchRequest.charAt(searchRequest.length - 1) === "?"
          ? (searchRequest += "artistOrCulture=true&q=" + this.searchObj.medium)
          : (searchRequest += "&artistOrCulture=true&q=" + this.searchObj.medium);
      }
      if (this.searchObj.searchTerm) {
        searchRequest.charAt(searchRequest.length - 1) === "?"
          ? (searchRequest += "q=" + this.searchObj.searchTerm)
          : (searchRequest += "&q=" + this.searchObj.searchTerm);
      }
      axios.get(searchRequest).then((resp) => this.getSearchResults(resp.data));
    },
    getSearchResults(results) {
      //limit for 20 search results for now
      for (let i = 0; i < 20; i++) {
        axios
          .get("https://collectionapi.metmuseum.org/public/collection/v1/objects/" + results.objectIDs[i])
          .then(resp => {
            this.searchResults.push(resp);
          })
      }
    }
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
