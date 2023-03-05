<template>
  <div id="page-wrap" v-if="product">
    <div id="img-wrap">
      <img v-bind:src="product.imageUrl" />
    </div>
    <div id="product-details">
      <h1>{{ product.hame }}</h1>
      <h3 id="price">${{ product.price }} </h3>
      <p>Average rating: {{ product.averageRating }}</p>
      <button 
      id="add-to-cart"
      v-if="!showSuccessMessage && !itemIsInCart"
      v-on:click="addToCart"
      >Add to cart</button>
      <button 
      id="add-to-cart"
      class="grey-button"
      v-if="itemIsInCart"
      >Item alredy in cart</button>
      <button 
      id="add-to-cart"
      class="green-button"
      v-if="showSuccessMessage && !itemIsInCart"
      >Successfully added item to cart!</button>
      <h4>Description</h4>
      <p>{{ product.description }}</p>
    </div>
  </div>
  <not-found-page v-else></not-found-page>
</template>

<script>
import axios from 'axios';
import NotFoundPage from './NotFoundPage.vue';
export default {
    name:'ProductDetailPage',
    components:{
      NotFoundPage,
    },
    data(){
      return{
        product:{},
        cartItems: [],
        showSuccessMessage: false,
      };
    },
    computed:{
      itemIsInCart (){
        return this.cartItems.some(item=> item.id===this.product.id);
      }
    },
    methods: {
      async addToCart() {
        await axios.post('/api/users/12345/cart', {
          productId: this.$route.params.id,
        });
        this.showSuccessMessage = true;
        setTimeout(()=>{
          this.$router.push('/products');
        },1500);
      },
    },
    async created(){
      const result = await axios.get(`/api/products/${this.$route.params.id}`);
      const product = result.data;
      this.product = product;

      const {data :cartItems} = await axios.get('/api/users/12345/cart')
      this.cartItems=cartItems;
    },
}
</script>

<style scoped>
  #page-wrap {
    margin-top: 16px;
    padding: 16px;
    max-width: 600px;
  }

  #img-wrap {
    text-align: center;
  }

  img {
    width: 400px;
  }

  #product-details {
    padding: 16px;
    position: relative;
  }

  #add-to-cart {
    width: 100%;
  }

  #price {
    position: absolute;
    top: 24px;
    right: 16px;
  }
  .green-button{
    background-color: green;
  }
  .grey-button{
    background-color:grey ;
  }
</style>