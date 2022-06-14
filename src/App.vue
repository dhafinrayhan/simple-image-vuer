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

  computed: {
    selectedImage() {
      return this.images[this.selectedIndex];
    }
  },

  methods: {
    async fetchData() {
      this.images = await (await fetch(API_URL)).json();
    },
    incrementIndex() {
      if (this.selectedIndex < this.images.length - 1) this.selectedIndex++;
    },
    decrementIndex() {
      if (this.selectedIndex > 0) this.selectedIndex--;
    }
  }
}
</script>

<template>
  <div v-if="this.selectedIndex !== null" class="image-view-group">
    <button @click="this.selectedIndex = null">X</button>
    <img :src="this.selectedImage.url" class="image-view">
    <p>{{ this.selectedImage.url }}</p>
  </div>
  <div class="images-list">
    <button v-if="this.selectedIndex !== null" @click="decrementIndex">&#8592;</button>
    <ul>
      <li v-for="(image, index) in images">
        <img :src="image.url" @click="this.selectedIndex = index" class="image-thumbnail"
          :class="{ 'image-thumbnail-selected': this.selectedIndex === index }">
      </li>
    </ul>
    <button v-if="this.selectedIndex !== null" @click="incrementIndex">&#8594;</button>
  </div>
</template>

<style>
body {
  font-family: sans-serif;
  background-color: #333;
  color: #eee;
}

.image-view-group {
  height: 400px;
  text-align: center;
  position: absolute;
  top: 8px;
  left: 0;
  right: 0;
}

.image-view {
  height: 100%;
  padding: 24px;
  display: block;
  margin-left: auto;
  margin-right: auto;
}

.image-thumbnail {
  width: 120px;
  height: 72px;
  margin: 2px;
  object-fit: cover;
  border: 2px solid #eee;
  border-radius: 12px;
}

.image-thumbnail-selected,
.image-thumbnail:hover {
  width: 144px;
  height: 88px;
  object-fit: cover;
  border: 4px solid cadetblue;
}

.image-thumbnail-selected {
  border: 4px solid royalblue;
}

.images-list {
  text-align: center;
  position: absolute;
  bottom: 0;
  left: 0;
  right: 0;
}

ul {
  padding: 12px;
  list-style-type: none;
  display: inline-flex;
}

li {
  margin-top: auto;
  margin-bottom: auto;
}

button {
  background-color: royalblue;
  font-size: 20px;
  color: white;
  border: none;
  border: 2px solid #ddd;
  border-radius: 8px;
}

button:hover {
  background-color: olive;
}
</style>
