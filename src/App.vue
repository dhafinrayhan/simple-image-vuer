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
  <div v-if="this.selectedIndex !== null" class="image-view-group">
    <button @click="this.selectedIndex = null">X</button>
    <img :src="this.selectedImage.url" class="image-view">
    <p>{{ this.selectedImage.url }}</p>
  </div>
  <div class="images-list">
    <button v-if="this.selectedIndex !== null" @click="decreaseIndex">left</button>
    <ul>
      <li v-for="(image, index) in images">
        <img :src="image.url" @click="this.selectedIndex = index" class="image-thumbnail"
          :class="{ 'image-thumbnail-selected': this.selectedIndex === index }">
      </li>
    </ul>
    <button v-if="this.selectedIndex !== null" @click="increaseIndex">right</button>
  </div>
</template>

<style>
body {
  background-color: #333;
  color: #eee;
}

.image-view-group {
  height: 400px;
  text-align: center;
}

.image-view {
  height: 320px;
  display: block;
  margin-left: auto;
  margin-right: auto;
}

.image-close-button {
  text-align: right;
}

.image-thumbnail {
  width: 144px;
  height: 80px;
  margin: 2px;
  object-fit: cover;
  border-radius: 12px;
}

.image-thumbnail-selected,
.image-thumbnail:hover {
  width: 160px;
  height: 96px;
  object-fit: cover;
  border: 4px solid green;
}

.image-thumbnail-selected {
  border: 4px solid blue;
}

.images-list {
  text-align: center;
  position: absolute;
  bottom: 0;
  left: 0;
  right: 0;
}

ul {
  list-style-type: none;
  display: inline-flex;
}

li {
  margin-top: auto;
  margin-bottom: auto;
}
</style>
