<template>
    <div class="carousel">
        <slot></slot>
        <button class="carousel__nav carousel__prev" @click.prevent="prev">Précédente</button>
        <button class="carousel__nav carousel__next" @click.prevent="next">Suivante</button>
        <div class="carousel__pagination">
            <button v-for="n in slidesCount" :class="{active: n - 1 === index}" @click.prevent="goto(n-1)" :key="n"></button>
        </div>
    </div>
</template>

<script>
export default {
    data() {
        return {
            index: 0,
            slides: [],
            direction: null
        }
    },
    methods: {
        prev() {
            this.index--
            this.direction ='left'
            if(this.index < 0) {
                this.index = this.slidesCount - 1
            }
        },
        next() {
            this.index++
            this.direction ='right'
            if(this.index > this.slidesCount - 1) {
                this.index = 0
            }
        },
        goto(index) {
            this.direction = index > this.index ? 'right' : 'left'
            this.index = index
        }
    },
    watch: {
        slides(slides) {
            if(this.index >= this.slidesCount) {
                this.index = this.slidesCount - 1
            }
        }
    },
    computed: {
        slidesCount() {
            return this.slides.length
        }
    },
    mounted() {
        this.slides = this.$children
    }
}
</script>

<style>

.carousel {
    position: relative;
}

.carousel__pagination {
    position: absolute;
    bottom: 20px;
    left: 0;
    right: 0;
    text-align: center;
}

.carousel__pagination button {
    display: inline-block;
    width: 10px;
    height: 10px;
    background-color: #fff;
    border-radius: 10px; 
    margin: 5px
}

.carousel__pagination button.active {
    background-color: #159;
}

</style>