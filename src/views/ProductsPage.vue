<template>
    <div id="page-wrap">
      <products-grid :products="products" />
    </div>
</template>

<script>
import axios from 'axios';
import ProductsGrid from '@/components/ProductsGrid.vue';
export default {
  components: { ProductsGrid },
    name:'ProductsPage',
    data(){
        return{
            products: [],
        };
    },
    async created(){
      const result = await axios.get('/api/products');
      const products = result.data;
      this.products = products;
    },
};
</script>

<style scoped>
</style>