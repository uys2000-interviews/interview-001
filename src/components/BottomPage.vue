<template>

  <q-tab-panels :model-value="page" animated class="shadow-2 rounded-borders">
    <q-tab-panel name="1">
      <page-left />
    </q-tab-panel>

    <q-tab-panel name="0">
      <page-center />
    </q-tab-panel>

    <q-tab-panel name="-1">
      <page-right />
    </q-tab-panel>
  </q-tab-panels>
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
  props: ["page"],
};
</script>