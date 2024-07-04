<template>
    <div>
      <div class="d-flex align-items-end gap-3 flex-wrap">
        <div>
          <input type="text" class="form-control" v-model="searchTerm" placeholder="Search for a lesson...">
        </div>
        <div>
          <label for="sortAttribute">Sort By:</label>
          <select v-model="sortAttribute" class="form-select">
            <option value="title">Title</option>
            <option value="location">Location</option>
            <option value="price">Price</option>
            <option value="spaces">Spaces</option>
          </select>
        </div>
        <div>
          <label for="sortOrder">Sort Order:</label>
          <select v-model="sortOrder" class="form-select">
            <option value="asc">Ascending</option>
            <option value="desc">Descending</option>
          </select>
        </div>
      </div>
      <div class="row">
        <div class="col-sm-6" v-for="product in sortedClasses" :key="product.id">
          <div class="card mb-3 mt-3 rounded border border-success-subtle">
            <div class="card-header d-flex">
              <div>
                <img class="img-fluid" style="width: 150px; height: 150px;" :src="product.image" alt="">
              </div>
              <i :class="product.icon" class="fs-1 ms-auto"></i>
            </div>
            <div class="card-body">
              <p>Title: {{ product.title }}</p>
              <p>Location: {{ product.location }}</p>
              <p>Spaces: {{ product.spaces }}</p><br>
              <p>Price: {{ product.price }} AED</p>
            </div>
            <div class="card-footer">
              <button class="btn btn-secondary rounded" @click="addToCart(product)" :disabled="product.spaces === 0">
                Add to Cart
              </button><br><br>
              <button class="btn btn-danger rounded" @click="removeProduct(product)">
                Remove
              </button>
            </div>
          </div>
        </div>
      </div>
    </div>
  </template>
  
  <script>
  export default {
    props: ['classes', 'cart'],
    data() {
      return {
        searchTerm: '',
        sortAttribute: 'title',
        sortOrder: 'asc',
      };
    },
    methods: {
      addToCart(product) {
        this.$emit('add-to-cart', product);
      },
      removeProduct(product) {
        this.$emit('remove-product', product);
      },
    },
    computed: {
      sortedClasses() {
        return this.classes
          .filter(item => item.title.toLowerCase().includes(this.searchTerm.toLowerCase()) || item.location.toLowerCase().includes(this.searchTerm.toLowerCase()))
          .sort((a, b) => {
            const valueA = a[this.sortAttribute];
            const valueB = b[this.sortAttribute];
            const comparison = typeof valueA === 'string' ? valueA.localeCompare(valueB) : valueA - valueB;
            return this.sortOrder === 'asc' ? comparison : -comparison;
          });
      },
    },
  };
  </script>
  
  <style scoped>
  /* Scoped styles for this component */
  </style>
  