<template>
    <div class="home">
      <h1>All Products</h1>

      <div class="row">
        <div class="col-md-4" v-for="product in products">
          <div class="card" style="width: 250px;"> 
            <h2>{{ product.name }}</h2>
            <router-link v-bind:to="'/products/' + product.id">
            <img class="card-img-top" v-bind:src="product.image_url" v-bind:alt="product.name">
            </router-link>
          </div>
          </div>
        </div>
      </div>
    </div>
</template>

<style>
</style>

<script>
var axios = require('axios');


export default {
  data: function() {
    return {
      products: [],
      currentProduct: {}
    };
  },
  created: function() {
    axios.get("/api/products")
      .then(response => {
        this.products = response.data;
      });
  },
  methods: {
  }
};
</script>



