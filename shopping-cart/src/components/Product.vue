<template>
  <div class="item" :class="{ outOfStock: currentlyInStock === 0 }">
    <h3>{{item.name}}</h3>
    <img :src="item.image_url">
    <p>{{item.description}}</p>
    <small class="in-stock">{{currentlyInStock}} in stock!</small>
    <br>
    <small class="price">{{formattedPrice}}</small>
    <div class="actions" v-if="currentlyInStock">
      <select v-model="selectedQuantity">
        <option
          v-for="quantity in currentlyInStock"
          :key="quantity"
          :value="quantity"
        >
          {{quantity}}
        </option>
      </select>
      <button @click="addToCart">Add to Cart</button>
    </div>
  </div>
</template>

<script>
export default {
  props: ['item', 'cartItem'],
  data() {
    return {
      selectedQuantity: 1,
    };
  },
  computed: {
    formattedPrice() {
      return '$' + (this.item.price / 100).toFixed(2)
    },
    currentlyInStock() {
      if (this.cartItem) {
        return this.item.in_stock - this.cartItem.quantity;
      }
      return this.item.in_stock;
    }
  },
  methods: {
    addToCart() {
      this.$emit('addToCart', this.item, this.selectedQuantity);
      this.selectedQuantity = 1;
    }
  }
};
</script>

<style scoped>
.item {
  display: block;
  border: 2px solid black;
  max-width: 500px;
  margin: 1rem auto;
  padding: 1rem;
  border-radius: 1rem;
}

.item h3 {
  text-align: center;
}

.item img {
  width: 100%;
  display: block;
  margin: 0 auto;
}

.item .in-stock {
  display: block;
  text-align: right;
}

.item .price {
  display: block;
  text-align: center;
  font-weight: bold;
  font-size: 3rem;
}

.item .actions {
  margin: 0;
  padding: 0;
}

.item button, .item select {
  width: 100%;
  margin: 0;
  padding: 0.5rem;
  border-radius: 1rem;
}

.outOfStock {
  text-decoration: line-through;
  filter: grayscale(1);
}

.outOfStock img {
  width: 20%;
}
</style>