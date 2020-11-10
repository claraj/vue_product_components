<template>
    <div>  <!-- add a div -->
    
  <h1>{{ product.productName }}</h1>

        <img v-bind:src="productApiUrl + 'img/' + product.image">

        <p>{{ product.description }}</p>

        <p v-if="product.available">This product is available!</p>
        <p v-else>Sorry, this product is not available. Please check back later.</p>

        <p>Quantity Available: {{ product.quantityAvailable }}</p>

        <ul>
            <li v-for="feature in product.features">{{ feature }}</li>
        </ul>

        <div>
            <div>
                <p>The maximum quantity you can order is {{ product.maxQuantity }}</p>
                <button v-on:click="decreaseQuantity(product)">-</button>
                <input type="number" v-model="product.quantity" v-on:change="verifyQuantity(product)">
                <button v-on:click="increaseQuantity(product)">+</button>
            </div>
            <p class="error">{{ product.message }}</p>
    </div>

    <button v-on:click="order(product)">Order</button>
 
    </div>   <!-- End of div -->
</template>  


<script>
export default {
    name: 'Product',
    // props - data that something else - another component - provides
    props: {
        product: Object,    // name: Type
        productApiUrl: String   // add new prop
    },
    // data that the component generates
    data() {
        return {
            // Vue component data 
            // must be a function 
        }
    }
}
</script>

<style scoped>
    img {
        height: 300px;
    }
</style>