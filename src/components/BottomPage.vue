<template>
  <page-left />
  <page-center />
  <page-right />
</template>


<script>
import pageLeft from "../Pages/PageLeft.vue";
import pageCenter from "../Pages/PageCenter.vue";
import pageRight from "../Pages/PageRight.vue";
export default {
  components: {
    pageLeft,
    pageCenter,
    pageRight,
  },
};
</script>