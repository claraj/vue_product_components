<template>
  <div id="app">
     <!-- Delete contents from here  leave the div id="app" -->
    <h1>Shopping App</h1>  <!-- add a title -->
    <!-- to start with - display the first product -->
    <!-- <product v-bind:product="products[0]"></product> -->
    
    <p v-for="order in orders">
      Product ID: {{ order.id }}  Quantity: {{ order.quantity }}
    </p>

    <!-- loop - display all the products -->
    <product 
      v-for="productData in products" 
      v-bind:key="productData.id" 
      v-bind:product="productData"
      v-bind:productApiUrl="productApiUrl"
      v-on:product-ordered="productOrdered"
      >
    </product>

  </div>
</template>

<script>
import Product from './components/Product.vue'

export default {
  name: 'App',
  components: {
    Product  // change this name to component's name 
  },
  data() {
    return {
      // copy this from products.html
      productApiUrl: 'https://vue-2560-product.herokuapp.com/',
      products: [],  // this will be set by the API call in mounted
      orders: []  // new orders array
    }
  },
  mounted() {    // copy from products.html 
    let productsURL = this.productApiUrl + 'api/products'
    fetch(productsURL)
        .then( response => response.json())
        .then( products => {
            this.products = products  // set Vue data to response from API
        })
  },
  methods: {
    productOrdered(productId, quantity) {
      let orderItem = { id: productId, quantity: quantity}
      this.orders.push(orderItem)
    }
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
