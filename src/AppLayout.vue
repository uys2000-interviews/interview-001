<template>
  <div>
    <upper-page @setPage="setPage"/>
  </div>
  <div>
    <bottom-page :page="pageS"/>
  </div>
</template>

<script>
import upperPage from "./components/UpperPage.vue";
import bottomPage from "./components/BottomPage.vue";
export default {
  components: {
    upperPage,
    bottomPage,
  },
  data() {
    return {
      page: 0,
      pageS: "0"
    };
  },
  provide() {
    return {
      page: this.page,
    };
  },
  methods: {
    setPage: function (page) {
      this.pageS = page.toString();
    },
  },
};
</script>