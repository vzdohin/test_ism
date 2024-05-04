<template>
  <div id="main">
    <div class="grid" v-for="image in images" :key="image.id">
      <img
        :src="image.webformatURL"
        @click="openImage(image.largeImageURL)"
        class="image"
      />
      <div class="info">
        <span>❤️ {{ image.likes }}</span>
        <span>👁️ {{ image.views }}</span>
      </div>
    </div>
    <transition name="fade">
      <div v-if="largeImage" class="overlay" @click="closeImage">
        <img :src="largeImage" class="overlay-image" />
      </div>
    </transition>
  </div>
</template>

<script>
import axios from "axios";

export default {
  data() {
    return {
      images: [],
      largeImage: null,
    };
  },
  async created() {
    const response = await axios.get(
      "https://pixabay.com/api/?key=43702940-5d8fd27b9937ad34af5745cf0&per_page=20"
    );
    this.images = response.data.hits;
  },
  methods: {
    openImage(url) {
      this.largeImage = url;
    },
    closeImage() {
      this.largeImage = null;
    },
  },
};
</script>

<style>
#main {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(200px, 1fr));
  gap: 10px;
}
.grid {
  display: flex;
  flex-direction: column;
}
.image {
  width: 100%;
  height: 200px;
  object-fit: cover;
  cursor: pointer;
  background-color: rgb(219, 219, 219);
}
.info {
  display: flex;
  justify-content: space-between;
}
.overlay {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background: rgba(0, 0, 0, 0.8);
  display: flex;
  justify-content: center;
  align-items: center;
  cursor: pointer;
}
.overlay-image {
  max-width: 90%;
  max-height: 90%;
}
.fade-enter-active,
.fade-leave-active {
  transition: opacity 0.5s;
}
.fade-enter-from,
.fade-leave-to {
  opacity: 0;
}
.fade-enter-to,
.fade-leave-from {
  opacity: 1;
}
@media screen and (max-width: 600px) {
  #app {
    grid-template-columns: 1fr;
  }
}
@media screen and (min-width: 601px) and (max-width: 1024px) {
  #app {
    grid-template-columns: repeat(2, 1fr);
  }
}
@media screen and (min-width: 1025px) and (max-width: 1440px) {
  #app {
    grid-template-columns: repeat(3, 1fr);
  }
}
@media screen and (min-width: 1441px) {
  #app {
    grid-template-columns: repeat(4, 1fr);
  }
}
</style>
