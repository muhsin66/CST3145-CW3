<template>
  <div id="app">
    <main class="container mb-8">
      <h1 class="text-center">MNH AfterClasses</h1>

      <!-- Header with cart button -->
      <div class="d-flex">
        <button class="btn btn-secondary fs-3 p-3 ms-auto" @click="toggleCheckout" :disabled="cartSize === 0 && showProduct">
          {{ cartSize }} <i class="bi bi-bag-check-fill"></i>
        </button>
      </div>

      <!-- Conditional rendering based on showProduct -->
      <div class="container">
        <ProductPage v-if="showProduct" :classes="filteredClasses" :cart="cart" @add-to-cart="addToCart" @remove-product="removeProduct"></ProductPage>
        <CheckoutPage v-else :cart="cart" @toggle-checkout="toggleCheckout"></CheckoutPage>
      </div>
    </main>
  </div>
</template>

<script>
import ProductPage from './components/ProductPage.vue';
import CheckoutPage from './components/CheckoutPage.vue';
import classes from './components/classes.js';

export default {
  name: 'App',
  components: {
    ProductPage,
    CheckoutPage,
  },
  data() {
    return {
      showProduct: true,
      classes: classes,
      cart: [],
      searchTerm: '',
      username: '',
      phonenumber: '',
      sortAttribute: 'title',
      sortOrder: 'asc',
    };
  },
  methods: {
    addToCart(classes) {
      if (classes.spaces > 0) {
        classes.spaces--;
        const cartItem = this.cart.find(item => item.classes === classes);
        if (cartItem) {
          cartItem.amount++;
        } else {
          this.cart.push({ classes: classes, amount: 1 });
        }
      }
    },
    removeProduct(classes) {
      const index = this.cart.findIndex(item => item.classes === classes);
      if (index !== -1) {
        this.cart[index].amount--;
        classes.spaces++;
        if (this.cart[index].amount === 0) {
          this.cart.splice(index, 1);
        }
      }
    },
    toggleCheckout() {
      this.showProduct = !this.showProduct;
    },
    // Method to sort classes based on current sort settings
    sortClasses() {
      return this.filteredClasses.slice().sort((a, b) => {
        const valueA = a[this.sortAttribute];
        const valueB = b[this.sortAttribute];
        const comparison = typeof valueA === 'string' ? valueA.localeCompare(valueB) : valueA - valueB;
        return this.sortOrder === 'asc' ? comparison : -comparison;
      });
    },
  },
  computed: {
    // Filter classes based on search term
    filteredClasses() {
      const searchTerm = this.searchTerm.toLowerCase();
      return this.classes.filter(item =>
        item.title.toLowerCase().includes(searchTerm) || item.location.toLowerCase().includes(searchTerm)
      );
    },
    // Use method for sorted classes instead of computed property
    sortedClasses() {
      return this.sortClasses();
    },
    cartSize() {
      return this.cart.reduce((total, item) => total + item.amount, 0);
    },
  },
};
</script>

<style scoped>
/* Scoped styles for this component */
* {
  text-transform: capitalize;
}
</style>
