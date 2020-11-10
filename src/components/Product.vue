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
                <!-- v-model quantity instead product.quantity -->
                <input type="number" v-model="quantity" v-on:change="verifyQuantity(product)">
                <button v-on:click="increaseQuantity(product)">+</button>
            </div>
            <!-- show the message -->
            <p class="error">{{ message }}</p>
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
            message: '',
            quantity: 0   // two data items
        }
    },  // don't forget the comma
    methods: {
        order(product) {
           this.$emit('product-ordered', this.product.id, this.quantity)
        },
        decreaseQuantity(product) {   // include product argument
            this.message = ''   // product.message, change to this.message
            let newQuantity = this.quantity - 1  // product.quantity change to this.quantity
            if ( newQuantity < product.minQuantity) {
                this.message = 'Minimum quantity is ' + product.minQuantity
            }
            else {
                this.quantity = newQuantity
            }
        },
        increaseQuantity(product) {
            this.message = ''
            let newQuantity = this.quantity + 1
            if ( newQuantity > product.maxQuantity) {
                this.message = 'Maximum quantity is ' + product.maxQuantity
            }
            else {
                this.quantity = newQuantity
            }      
        },
        verifyQuantity(product) {
            this.message = ''
            if (this.quantity < product.minQuantity ) {
                this.message = 'Minimum quantity is ' + product.minQuantity
                this.quantity = product.minQuantity
            }
            if (this.quantity > product.maxQuantity) {
                this.message = 'Maximum quantity is ' + product.maxQuantity
                this.quantity = product.maxQuantity
            }
        }
    }
}
</script>

<style scoped>
    img {
        height: 300px;
    }

    .error {
        color: red;
        font-weight: bold;
    }
</style>