<template>
  <div class="products-new">
    <h1>New Products</h1>

    <ul>
      <li v-for="error in errors">{{ error }}</li>
    </ul>

    <form v-on:submit.prevent="submit()">
    <div>
      Name: <input v-model="newProductName">
    </div>
     <div>
      Price: <input v-model="newProductPrice">
    </div>
     <div>
      Description: <input v-model="newProductDescription">
    </div>
     <div>
      Image URL: <input v-model="newProductImageUrl">
    </div>
    <div>
      <input type="submit" value="Create">
    </div>
    </form>
  </div>
</template>

<style>
</style>

<script>
var axios = require('axios');

export default {
  data: function() {
    return {
      newProductName: "",
      newProductPrice: "",
      newProductDescription: "",
      newProductImageUrl: "",
      errors: []
    };
  },
  created: function() {},
  methods: {
    submit: function() {
      console.log("Create the Product...");
      var params = {
                    name: this.newProductName,
                    price: this.newProductPrice,
                    description: this.newProductDescription,
                    image_url: this.newProductImageUrl
                    };
      axios.post("/api/products", params)
      .then(response => {
        console.log("Success", response.data);
        this.$router.push('/');
      }).catch(error => {
        this.errors = error.response.data.errors;
      });
    }
  }
};
</script>