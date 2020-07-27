<template>
  <table class="table">
    <thead>
      <th>Name</th>
      <th>Price</th>
      <th>Quantity</th>
      <th>Total</th>
      <th>Actions</th>
    </thead>
    <tbody>
      <tr v-for="cartItem in items" :key="cartItem.item.id">
        <td>{{cartItem.item.name}}</td>
        <td>{{formatPrice(cartItem.item.price)}}</td>
        <td><input min="1" :max="cartItem.item.in_stock" type="number" v-model="cartItem.quantity"></td>
        <td>{{formatPrice(cartItem.quantity * cartItem.item.price)}}</td>
        <td><button @click="$emit('removeItem', cartItem)">Remove</button></td>
      </tr>
      <tr>
        <td></td>
        <td><strong>Total:</strong></td>
        <td>{{totalItems}}</td>
        <td>{{totalPrice}}</td>
      </tr>
    </tbody>
  </table>
</template>

<script>
export default {
  props: ['items'],
  computed: {
    totalItems() {
      let total = 0;
      this.items.forEach(({ quantity }) => {
        total += quantity;
      });
      return Number(total);
    },
    totalPrice() {
      let price = 0;
      this.items.forEach(({ item, quantity }) => {
        price += quantity * item.price;
      });
      return this.formatPrice(price);
    }
  },
  methods: {
    formatPrice(price) {
      return '$' + (price / 100).toFixed(2)
    },
  }
};
</script>

<style scoped>
.table {
  width: 100%;
}
.table, td, tr, th {
  border: 1px solid grey;
  padding: 0.25rem;
  width: 100%;
}

button {
  margin: 0;
  padding: 0;
}
</style>