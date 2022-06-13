<script>
const API_URL = `https://api.thecatapi.com/v1/images/search?limit=5&page=1`

export default {
  data() {
    return {
      images: [],
      selectedIndex: null,
    }
  },

  created() {
    this.fetchData();
  },

  methods: {
    async fetchData() {
      this.images = await (await fetch(API_URL)).json();
    },
    increaseIndex() {
      if (this.selectedIndex < this.images.length - 1) this.selectedIndex++;
    },
    decreaseIndex() {
      if (this.selectedIndex > 0) this.selectedIndex--;
    }
  }
}
</script>

<template>
  <button v-if="this.selectedIndex !== null" @click="this.selectedIndex = null">X</button>
  <img v-if="this.selectedIndex !== null" :src="this.images[selectedIndex].url" class="image-view">
  <p v-if="this.selectedIndex !== null">{{ this.images[this.selectedIndex].url }}</p>
  <button v-if="this.selectedIndex !== null" @click="decreaseIndex">left</button>
  <button v-if="this.selectedIndex !== null" @click="increaseIndex">right</button>
  <ul>
    <li v-for="(image, index) in images">
      <img :src="image.url" @click="this.selectedIndex = index" class="image-thumbnail"
        :class="{ 'image-thumbnail-selected': this.selectedIndex === index }">
    </li>
  </ul>
</template>

<style>
.image-view {
  height: 320px;
  display: block;
  margin-left: auto;
  margin-right: auto;
}

.image-thumbnail {
  width: 144px;
  height: 80px;
  object-fit: cover;
}

.image-thumbnail-selected,
.image-thumbnail:hover {
  width: 160px;
  height: 96px;
  object-fit: cover;
  border: 4px solid blue;
}

ul {
  list-style-type: none;
}

li {
  float: left;
}
</style>
