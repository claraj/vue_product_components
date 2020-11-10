<template>
  <div id="app">

     <!-- Delete contents from the hello world page, leave the div id="app" -->
    <h1>Shopping App</h1>  <!-- add a title -->
    
    <!-- Order component, v-bind the orders array -->
    <order v-bind:orders="orders"></order>

    <h2>Product Catalog</h2>

    <!-- to start with - display the first product -->
    <!-- <product v-bind:product="products[0]"></product> -->
   
    <!-- loop - display all the products --> 
    <product 
      v-for="productData in products" v-bind:key="productData.id" 
      v-bind:product="productData"
      v-bind:productApiUrl="productApiUrl"
      v-on:product-ordered="productOrdered" ></product>

  </div>
</template>

<script>
import Product from './components/Product.vue'
import Order from './components/Order.vue'

export default {
  name: 'App',
  components: {
    Product, Order   // list all the component's names 
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

      // look up name and other product info in products array 
      let productInfo = this.products.find( p => p.id == productId)

      // new object with info about this order - the product object and quantity
      let orderItem = {product: productInfo, quantity: quantity}

      // avoid duplicate orders for the same product 

      // Has a product with this ID already been ordered?
      // search the orders array for an order with this product ID
      let existingOrderIndex = this.orders.findIndex( order => order.product.id == productId)
      if (existingOrderIndex == -1 ) {
        // add to orders array
        this.orders.push(orderItem)
      }
      else {
        // Vue has a hard time tracking modifications to objects within arrays
        // so this looks like it would work,
        // this.orders[existingOrderIndex] = orderItem 
        // and it would update the orders array but the page wouldn't change.
        // so we have to use the $set method to make the change to the array
        // and make the page update.
        this.$set(this.orders, existingOrderIndex, orderItem)
      }

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
