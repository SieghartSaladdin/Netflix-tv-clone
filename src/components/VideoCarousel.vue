<script setup>
import { ref, toRefs, watch } from 'vue';
import 'vue3-carousel/dist/carousel.css';
import { Carousel, Slide, Navigation } from 'vue3-carousel';

import { useMovieStore } from '../stores/movie'
import { storeToRefs } from 'pinia';
const useMovie = useMovieStore()
const { movie, showFullVideo } = storeToRefs(useMovie)

let currentSlide = ref(0);

const props = defineProps({
    category: String,
    movies: Array,
});

const { movies, category } = toRefs(props);

const currentSlideObject = (slide, index) => {
    if (index === currentSlide.value) {
        movie.value = slide
    }
}

const slidingClick = (index) => {
    currentSlide.value = index
}

const videoPreview = (slide) => {
    movie.value = slide; 
    showFullVideo.value = true
}

</script>


<template>
    <div class="min-w-[1200px] relative">
        <div class="flex justify-between mr-6">
            <div class="flex items-center font-semibold text-white text-2xl cursor-pointer">
                {{ category }}
            </div>
        </div>

        <Carousel
            ref="carousel"
            v-model="currentSlide"
            :items-to-scroll="1"
            :items-to-show="8"
            :wrap-around="true"
            :transition="500"
            snapAlign="start"
            class="bg-transparent"
        >
            <Slide
                v-for="(slide, index) in movies" :key="index"
                class="flex items-center object-cover text-white bg-transparent"
            >
                <div 
                    @click="slidingClick(index)"
                    class="object-cover h-[100%] hover:brightness-125 group cursor-pointer relative"
                    :class="currentSlide !== index ? 'border-4 border-transparent' : 'border-4 border-white', currentSlideObject(slide, index)"
                >
                    <img
                        style="user-select: none;"
                        class="pointer-events-none h-[100%] z-[-1]" 
                        :src="'/images/'+slide.name+'.png'"
                    >
                    <div 
                        class="bottom-14 hidden cursor-pointer group-hover:block left-1/2 transform -translate-x-1/2 absolute px-5 py-3 bg-black opacity-80 rounded-xl"
                        @click="videoPreview(slide)"
                    >
                        Preview
                    </div>
                </div>
            </Slide>
            <template #addons>
                <Navigation/>
            </template>
        </Carousel>
    </div>
</template>

<style>
    .carousel__prev,
    .carousel__next,
    .carousel__prev:hover,
    .carousel__next:hover {
        color: white;
    }
</style>