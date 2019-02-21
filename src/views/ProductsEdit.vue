<template>
  <div class="products-edit">
    <ul>
      <li v-for="error in errors">{{ error }}</li>
    </ul>

    <form v-on:submit.prevent="submit()">
    <div>
      Name: <input v-model="product.name">
    </div>
     <div>
      Price: <input v-model="product.price">
    </div>
     <div>
      Description: <input v-model="product.description">
    </div>
     <div>
      Image URL: <input v-model="product.image_url">
    </div>
    <div>
      <input type="submit" value="Update">
    </div>
    </form>
    
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
                  },
        errors: []
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
        submit: function() {
          var params = {
                        name: this.product.name,
                        price: this.product.price,
                        description: this.product.description,
                        image_url: this.product.image_url
                        };
          axios.patch( "/api/products/" + this.product.id, params )
          .then(response => {
            this.$router.push("/products/" + this.product.id);
          });
        }
      }
    }
</script>