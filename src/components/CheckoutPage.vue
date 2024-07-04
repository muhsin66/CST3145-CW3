<template>
    <div class="row g-3">
      <div class="col-sm-8 border rounded-3">
        <div v-for="item in cart" class="border-bottom" :key="item.classes.id">
          <div class="col-sm-12 mb-4 p-3">
            <div class="row">
              <div class="col-sm-4">
                <div>
                  <img :src="item.classes.image" style="width: 200px;" class="col-sm-8 border rounded-3">
                </div>
              </div>
              <div class="col-sm-4">
                <p>Title: {{ item.classes.title }}</p>
                <p>Quantity: {{ item.amount }}</p>
                <button class="btn btn-danger rounded" @click="removeProduct(item.classes)">
                  Remove
                </button>
              </div>
              <div class="col-sm-4">
                <p>Price: {{ item.classes.price }}</p>
              </div>
            </div>
          </div>
        </div>
      </div>
      <div class="col-sm-4">
        <div class="container">
          <div class="row justify-content-center">
            <div class="col-md-12">
              <div class="card">
                <div class="card-body">
                  <h5 class="card-title">User Information</h5>
                  <form @submit.prevent="submitOrder">
                    <div class="mb-3">
                      <label for="username" class="form-label">Username</label>
                      <input type="text" v-model="username" class="form-control" id="username" name="username" placeholder="Enter your username" required>
                    </div>
                    <div class="mb-3">
                      <label for="phone" class="form-label">Phone Number</label>
                      <input type="tel" v-model="phonenumber" class="form-control" id="phone" name="phone" placeholder="Enter your phone number" required>
                    </div>
                    <button type="submit" class="btn btn-info" :disabled="!validateUserCredentials">
                      Submit
                    </button>
                  </form>
                </div>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
  </template>
  
  <script>
  export default {
    props: ['cart'],
    data() {
      return {
        username: '',
        phonenumber: '',
      };
    },
    methods: {
      removeProduct(product) {
        this.$emit('remove-product', product);
      },
      submitOrder() {
        if (this.validateUserCredentials) {
          alert('Order Completed!');
          // Here you can implement further logic for submitting the order
        } else {
          alert('Please enter valid credentials.');
        }
      },
      validateUserCredentials() {
        return /^[a-zA-Z]+$/.test(this.username) && /^\d+$/.test(this.phonenumber);
      },
    },
  };
  </script>
  
  <style scoped>
  /* Scoped styles for this component */
  </style>
  