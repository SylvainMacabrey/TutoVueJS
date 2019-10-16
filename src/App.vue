<template>
  <div>
    <carousel>
      <carousel-slide v-for="n in slides" :index="n - 1">
        <img :src="'https://picsum.photos/id/' + n * n + '/800/400'" width="100%">
      </carousel-slide>
    </carousel>
    <button @click.prevent="addSlide">Ajouter un slide</button>
    <button @click.prevent="removeSlide">Supprimer un slide</button>
    <todos></todos>
  </div>
</template>

<script>
import Todos from './components/Todos'
import Carousel from './components/carousel/Carousel'
import CarouselSlide from './components/carousel/CarouselSlide'

export default {
  name: 'App',
  components: {
    Todos,
    Carousel,
    CarouselSlide
  },
  data() {
    return {
      slides: 5
    }
  },
  methods: {
    addSlide() {
      this.slides++
    },
    removeSlide() {
      this.slides--
    }
  }
}
</script>


