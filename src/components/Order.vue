<template>
    
<div>
  <p v-for="order in orders"> 
      Product ID: {{ order.id }}  Quantity: {{ order.quantity }}
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