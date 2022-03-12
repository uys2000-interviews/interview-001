<template>
  <upper-page :page="page" />
</template>

<script>
import upperPage from "./components/UpperPage.vue";
export default {
  components: {
    upperPage,
  },
  data() {
    return {
      page: 0,
    };
  },
};
</script>