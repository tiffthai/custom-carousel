<template>
  <div id="carouselWrapper">
    <Carousel 
      :slideData="slideData"
    />
  </div>
</template>

<script>
import Carousel from './components/Carousel.vue'

export default {
  name: 'App',
  components: {
    Carousel
  },
  data() {
    return {
      slideData: []
    }
  },
  created() {
    fetch('https://frontend-assessment-service.vcomm.io')
    .then(response => response.json())
    .then(val => {
      this.slideData = val.data;
    });
  }
}
</script>

<style>
html, body {
  margin: 0;
  width: 100%;
  height: 100%;
}

#app {
  width: 100%;
  height: 100%;
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  overflow: hidden;
}

#carouselWrapper {
  width: 100%;
  height: 100%;
  display: flex;
  justify-content: center;
  align-items: center;
  flex-direction: column;
}
</style>
