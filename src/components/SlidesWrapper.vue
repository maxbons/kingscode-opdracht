<template>
    <div class="carousel-view">
        <transition-group class="carousel" tag="div">
            <Slide
                v-for="(slide, index) in slides.slides"
                :key="index"
                :slideData="slide"
                class="slide"
            >
                <h1>{{ slide.title }}</h1>
            </Slide>
        </transition-group>
    </div>
</template>

<script lang="ts" setup>
// Imports
import slides from '@/data.json'
import Slide from '@/components/Slide.vue'
</script>

<style>
.carousel-view {
    display: flex;
    flex-direction: column;
    align-items: center;
}

.carousel {
    display: flex;
    justify-content: center;
    align-items: center;
    overflow: hidden;
    height: 100vh;
    width: 100vw;
}

.slide {
    /* flex: 0 0 20em; */
    display: flex;
    justify-content: center;
    align-items: center;
    transition: transform 0.3s ease-in-out;
}
</style>
