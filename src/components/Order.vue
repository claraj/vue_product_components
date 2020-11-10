<template>
    
<div>
  <p v-for="order in orders" v-bind:key="order.product.id"> 
      Product ID: {{ order.product.id }}  
      Name: {{ order.product.productName }} 
      Quantity: {{ order.quantity }}
  </p>
</div>

</template>

<script>
export default {
    name: 'Order',
    props: {
        orders: Array
    }
}
</script>

<style scoped>

</style>