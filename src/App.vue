<template>
  <div id="app">
     <!-- Delete contents from here  leave the div id="app" -->
    <h1>Shopping App</h1>  <!-- add a title -->
    <!-- to start with - display the first product -->
    <!-- <product v-bind:product="products[0]"></product> -->
    
    <order v-bind:orders="orders"></order>

    <!-- loop - display all the products -->
    <product 
      v-for="productData in products"  v-bind:key="productData.id" 
      v-bind:product="productData"
      v-bind:productApiUrl="productApiUrl"
      v-on:product-ordered="productOrdered" > </product>
  </div>
</template>

<script>
import Product from './components/Product.vue'
import Order from './components/Order.vue'

export default {
  name: 'App',
  components: {
    Product, Order   // change this name to component's name 
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
  methods: {   // new method 
    productOrdered(productId, quantity) {
      // look up name in products array 
      let productInfo = this.products.find( p => p.id == productId)
      let orderItem = { product: productInfo, quantity: quantity}
      
      let existingOrderIndex = this.orders.findIndex( o => o.product.id == productId)
      if (existingOrderIndex == -1 ) {
        this.orders.push(orderItem)
      }
      else {
        this.$set(this.orders, existingOrderIndex, orderItem)
      }
      // avoid duplicate orders for the same product 

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
