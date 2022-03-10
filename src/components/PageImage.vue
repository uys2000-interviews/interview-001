<template>
  <div>
    <q-img class="img" :src="src" />
  </div>
</template>

<script>
export default {
  props: ["src"],
};
</script>
<style>
.img {
  width: 100wv;
  height: 100vh;
}
</style>