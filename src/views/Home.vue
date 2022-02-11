<script>
import axios from "axios";

export default {
  data: function () {
    return {
      message: "Welcome to Vue.js!",
      places: [],
      currentPlace: {},
      editPlaceParams: {},
      createPlaceParams: {},
    };
  },
  created: function () {
    this.indexPlaces();
  },
  methods: {
    indexPlaces: function () {
      axios.get("/places").then((response) => (this.places = response.data));
    },
    createPlace: function () {
      axios
        .post("/places", this.createPlaceParams)
        .then((response) => {
          console.log(response.data);
          this.places.push(response.data);
        })
        .catch((error) => console.log(error.response.data.errors));
    },
    showPlace: function (place) {
      this.currentPlace = place;
      this.editPlaceParams = place;
      console.log(place);
      document.querySelector("#place-details").showModal();
    },
    updatePlace: function (place) {
      axios
        .patch(`/places/${place.id}`, place)
        .then((response) => {
          console.log(response.data);
        })
        .catch((error) => console.log(error.response.data.errors));
    },
    destroyPlace: function (place) {
      axios
        .delete(`/places/${place.id}`)
        .then((response) => console.log(response.data))
        .catch((error) => console.log(error.response.data.errors));
    },
  },
};
</script>

<template>
  <div class="home">
    <div>
      <h3>Create Place:</h3>
      <p>
        Name:
        <input type="text" v-model="createPlaceParams.name" />
      </p>
      <p>
        Address:
        <input type="text" v-model="createPlaceParams.address" />
      </p>
      <button v-on:click="createPlace()">Create</button>
    </div>
    <h3>All Places...</h3>
    <div v-for="place in places" :key="place.id">
      <p>{{ place.name }}</p>
      <p>{{ place.address }}</p>
      <button v-on:click="showPlace(place)">Show Info</button>
    </div>
    <dialog id="place-details">
      <form>
        <p>
          Name:
          <input type="text" v-model="editPlaceParams.name" />
        </p>
        <p>
          Address:
          <input type="text" v-model="editPlaceParams.address" />
        </p>
        <button v-on:click="updatePlace(editPlaceParams)">Update</button>
        <button v-on:click="destroyPlace(currentPlace)">Destroy</button>
        <button>Close</button>
      </form>
    </dialog>
  </div>
</template>

<style></style>
