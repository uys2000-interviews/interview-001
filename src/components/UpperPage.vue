<template>
  <div class="image image-left">
    <comp-img :src="require('@/assets/left.jpg')" />
  </div>
  <div class="image image-center">
    <comp-img :src="require('@/assets/center.jpg')" />
  </div>
  <div class="image image-right">
    <comp-img :src="require('@/assets/right.jpg')" />
  </div>
</template>

<script>
import compImg from "./PageImage.vue";
export default {
  components: {
    compImg,
  },
};
</script>