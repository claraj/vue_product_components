<template>
    
<div>

<h2>Your Orders</h2>

    <p v-if="orders.length==0">No orders</p>

    <table v-else>
        <tr>
            <th>Product ID</th>
            <th>Name</th>
            <th>Quantity</th>
            <th>Delete?</th>
        </tr>
        <tr v-for="order in sortedOrdersByName" v-bind:key="order.product.id"> 
            <td>{{ order.product.id }}</td>  
            <td>{{ order.product.productName }} </td>
            <td>{{ order.quantity }}</td>
            <td><button v-on:click="deleteOrder(order)">Delete</button></td>
        </tr>
    </table>

</div>

</template>

<script>
export default {
    name: 'Order',
    props: {
        orders: Array
    },
    computed: {
        sortedOrdersByName() {
            let copyOrders = [...this.orders]
            return copyOrders.sort( (o1, o2) => o1.product.name > o2.product.name ? -1 : 1)
        }
    },
    methods: {
        deleteOrder(order) {
            if (confirm(`Delete your order for ${order.quantity} ${order.product.productName}, are you sure?`)) {
                this.$emit('delete-order', order)
            }
        }
    }
}
</script>

<style scoped>

table {
    /* center the table - the text-align style in App.vue won't center it.  */
    margin-left: auto;
    margin-right: auto;
}

td, th {
    padding-left: 20px;
    padding-right: 20px;
}

table, th, td, tr {
    border: 1px lightgrey solid;
    border-collapse: collapse;
}

button {
    background: red;
}
</style>