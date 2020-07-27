<template>
  <div class="home">
    <h1>{{title}}</h1>
    <img v-if="loading" src="https://i.imgur.com/SQiriwO.gif">
    <div class="items" v-else>
      <div class="products">
        <Product
          v-for="item in items"
          :key="item.id"
          :item="item"
          :cartItem="cartItemsById[item.id]"
          @addToCart="addToCart"
        />
      </div>
      <div class="cart">
        <Cart :items="cartItems" @removeItem="removeFromCart" />
      </div>
    </div>
  </div>
</template>

<script>
import Product from '../components/Product';
import Cart from '../components/Cart';

export default {
  data() {
    return {
      loading: true,
      title: 'Sticker Store Products',
      items: [],
      cartItems: [],
    };
  },
  components: {
    Product,
    Cart,
  },
  created() {
    this.getProducts();
  },
  computed: {
    cartItemsById() {
      const itemsById = {};
      this.cartItems.forEach((cartItem) => {
        itemsById[cartItem.item.id] = cartItem;
      });
      return itemsById;
    },
  },
  methods: {
    async getProducts() {
      this.loading = true;
      const response = await fetch('https://sticker-store-api.vercel.app/api/items');
      const items = await response.json();
      this.items = items;
      this.loading = false;      
    },
    addToCart(item, quantity) {
      const existingItemInCart = this.cartItemsById[item.id];
      if (existingItemInCart) {
        existingItemInCart.quantity += quantity;
      } else {
        this.cartItems.push({
          item,
          quantity,
        });
      }
    },
    removeFromCart(cartItem) {
      this.cartItems = this.cartItems.filter((item) => item !== cartItem);
    }
  }
};
</script>

<style scoped>
h1 {
  text-align: center;
}

img {
  display: block;
  margin: 0 auto;
}

.items {
  display: flex;
}

.products {
  width: 50%;
}

.cart {
  width: 50%;
}
</style>