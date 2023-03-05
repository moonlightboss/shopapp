<template>
  <div class="grid-wrap">
    <products-grid-item
    v-for="product in products"
    v-bind:key="product.id"
    :product="product"
    ></products-grid-item>
  </div>
</template>

<script>
import ProductsGridItem from './ProductsGridItem.vue';

export default {
    name:'ProductsGrid',
    props:['products'],
  components: { ProductsGridItem },

}
</script>

<style scoped>
  .grid-wrap {
    display: flex;
    flex-wrap: wrap;
    justify-content: space-between;
    margin-top: 16px;
  }
</style>