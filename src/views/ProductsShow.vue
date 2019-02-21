<template>
  <div class="products-show">
    <h1>{{product.name}}</h1>
    <img v-bind:src="product.image_url" v-bind:alt="product.name">
    <h2>Price: {{ product.price }}</h2>
    <p>Description: {{product.description}} </p>
  <router-link class="btn btn-success" :to="'/products/' + product.id + '/edit' ">Edit</router-link>
  <button class="btn btn-danger" v-on:click="destroyProduct()">Delete</button>
  </div>
</template>

<script>
  var axios = require('axios');

  export default {
    data: function() {
      return {
        product: {
                  id: "",
                  name: "",
                  description: "",
                  price: "",
                  tax: "",
                  total: "",
                  formatted: {
                              price: "",
                              tax: "",
                              total: ""
                              }
                  }
      }
    },
      created: function() {
        axios.get("/api/products/" + this.$route.params.id)
        .then(response => {
          console.log(response.data);
          this.product = response.data;
        });
      },
      methods: {
        destroyProduct: function() {
          axios.delete("/api/products/" + this.product.id)
          .then(response => {
            console.log("Success", response.data);
            this.$router.push('/');
          }); 
        }
      }
    }
</script>