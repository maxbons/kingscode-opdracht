<template>
    <div ref="wrapper" class="wrapper">
        <div
            ref="slide"
            class="slide"
            :class="{ 'transition-all duration-100 ease-linear': !isSwiping }"
            :style="{ left, opacity }"
        >
            <slot></slot>
        </div>
    </div>
</template>

<script setup lang="ts">
// Imports
import { computed, ref } from 'vue'
import { usePointerSwipe } from '@vueuse/core'

// Props
defineProps<{
    slideData: {
        title: string
        direction: string
        level: number
    }
}>()

// Consts
const slide = ref<HTMLElement | null>(null)
const wrapper = ref<HTMLElement | null>(null)
const left = ref('0')
const opacity = ref(1)
const containerWidth = computed(() => wrapper.value?.offsetWidth)

// Functions
const { distanceX, isSwiping } = usePointerSwipe(slide, {
    disableTextSelect: true,
    onSwipe() {
        if (containerWidth.value) {
            if (distanceX.value < 0) {
                const distance = Math.abs(distanceX.value)
                left.value = `${distance}px`
                opacity.value = 1.25 - distance / containerWidth.value
            } else {
                left.value = '0'
                opacity.value = 1
            }
        }
    },
    onSwipeEnd() {
        if (
            distanceX.value < 0 &&
            containerWidth.value &&
            Math.abs(distanceX.value) / containerWidth.value >= 0.5
        ) {
            left.value = '100%'
            opacity.value = 0
        } else {
            left.value = '0'
            opacity.value = 1
        }
    }
})
</script>

<style scoped>
.wrapper {
    position: relative;
    height: 100vh;
    overflow: hidden;
    background: gray;
}

.slide {
    position: absolute;
    top: 0;
    left: 0;
    height: 100%;
    width: 100%;
    background: red;
    text-align: center;
}
</style>
