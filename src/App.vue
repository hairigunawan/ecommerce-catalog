<template>
  <ProductDisplay
    :product="product"
    :unavailable="unavailable"
    :nextProduct="nextProduct"
  />
</template>

<script>
  import ProductDisplay from './components/ProductDisplay.vue';
  export default {
    name: 'App',
    components: {
      ProductDisplay
    },
  };
</script>

<style scoped>

</style>