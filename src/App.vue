<script setup>
  import {ref, onMounted, onUnmounted} from 'vue'
  const currentIndex = ref(0)
  const images = [
    'https://images.unsplash.com/photo-1682685797769-481b48222adf?ixlib=rb-4.0.3&ixid=M3wxMjA3fDF8MHxlZGl0b3JpYWwtZmVlZHwxfHx8ZW58MHx8fHx8&auto=format&fit=crop&w=300&q=60',

    'https://images.unsplash.com/photo-1682695794816-7b9da18ed470?ixlib=rb-4.0.3&ixid=M3wxMjA3fDB8MHxwaG90by1yZWxhdGVkfDZ8fHxlbnwwfHx8fHw%3D&auto=format&fit=crop&w=300&q=60',

    'https://images.unsplash.com/photo-1682685797661-9e0c87f59c60?ixlib=rb-4.0.3&ixid=M3wxMjA3fDB8MHxwaG90by1yZWxhdGVkfDE4fHx8ZW58MHx8fHx8&auto=format&fit=crop&w=300&q=60',

    'https://plus.unsplash.com/premium_photo-1666963323736-5ee1c16ef19d?ixlib=rb-4.0.3&ixid=M3wxMjA3fDB8MHxwaG90by1yZWxhdGVkfDE2fHx8ZW58MHx8fHx8&auto=format&fit=crop&w=300&q=60',
  ]
  let interval;
  // this for next button to change carusal image
  const next = ()=>{
    currentIndex.value = ( currentIndex.value + 1 ) % images.length
  }
  // this for prev button to change carusal image
  const prev = ()=>{
    currentIndex.value = ( currentIndex.value - 1 + images.length) % images.length
  }
  // change image from indicator
  const indecatorImage = (index) => {
    currentIndex.value = index;
  };
  // carusal autoplay function
  const autoplay = () => {
    interval = setInterval(() => {
      next();
    }, 3000); // Adjust the interval (in milliseconds) as needed
  };
    // when mouse hover on image stop autoplay
  const stopAutoplay = () => {
    clearInterval(interval);
  };
  // when mouse leave on image stop autoplay
  const startAutoplay = () => {
    autoplay();
  };
    // autoplay mounded function
  onMounted(() => {
    autoplay();
  });
  // autoplay unmounted function
  onUnmounted(() => {
    clearInterval(interval);
  });

</script>
<template>
  <div class="relative" @mouseenter="stopAutoplay" @mouseleave="startAutoplay">
    <div v-for="(image, index) in images" :class="`carousel-item ${index === currentIndex ? 'active' : ''}`">
      <img :src="image" alt="Carousel Image" class="w-full h-auto rounded-lg shadow">
    </div>
    <button @click="prev" class="prev-btn">Prev</button>
    <button @click="next" class="next-btn">Next</button>

    <!-- footer indicator -->
    <div class="flex justify-center mt-">
        <span
          v-for="(image, index) in images"
          :key="index"
          @click="indecatorImage(index)"
          :class="`carousel-dot ${index === currentIndex ? 'active' : ''}`"
        ></span>
      </div>
  </div>
</template>
<style scoped>
 .carousel-item {
    display: none;
    transition: opacity 0.3s ease;
  }
  .carousel-item.active {
    display: block;
  }
  .prev-btn,
  .next-btn {
    position: absolute;
    top: 50%;
    transform: translateY(-50%);
    background-color: rgba(0, 0, 0, 0.5);
    color: white;
    padding: 0.5rem 1rem;
    border: none;
    border-radius: 4px;
    cursor: pointer;
    z-index: 1;
  }
  .prev-btn {
    left: 10px;
  }
  .next-btn {
    right: 10px;
  }
  .carousel-dot {
    width: 8px;
    height: 8px;
    background-color: #ccc;
    border-radius: 50%;
    margin: 0 4px;
    cursor: pointer;
  }
  .carousel-dot.active {
    background-color: #e60f0f;
  }
  .mt-{
    margin-top:-20px
  }
</style>
