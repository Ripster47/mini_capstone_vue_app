<template>
  <div class="home">
    <h1>New Products</h1>
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
      <button type="button" class="btn btn-light" v-on:click="createProduct()">Create</button>
    </div>

    <h1>All Products</h1>
    <div v-for="product in products">
      <h2>{{ product.name }}</h2>
      <img v-bind:src="product.image_url" v-bind:alt="product.name">
      <div>
        <button type="button" class="btn btn-light" v-on:click="currentProduct = product">More Info</button>
      </div>
      <div v-if="product === currentProduct">
        <p>Description: {{ product.description }}</p>
        <p>Price: {{ product.formatted.price }}</p>
        
        <div>
          <h4>Edit Recipe</h4>
          <div>
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
              <button type="button" class="btn btn-light" v-on:click="updateProduct(product)">Update</button>
              <button type="button" class="btn btn-light" v-on:click="destroyProduct(product)">Delete</button>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<style>

  h1{
    color: #00ff00;
    text-shadow: 3px 4px #3080ff
  }

  h2{
    color: #ddddff;
    text-shadow: 3px 4px #0000ff
  }

  p{
    color: #FFFFFF;
  }

  body{
    background-image: url("https://images.unsplash.com/photo-1527492662722-dbaf97270863?ixlib=rb-1.2.1&ixid=eyJhcHBfaWQiOjEyMDd9&w=1000&q=80");
    background-repeat: no-repeat;
    background-attachment: fixed;
    height: 100%; 

    /* Center and scale the image nicely */
    background-position: center;
    background-repeat: no-repeat;
    background-size: cover;
  }

  img{
    width: 250px;
  }
  </style>

<script>
var axios = require('axios');


export default {
  data: function() {
    return {
      products: [],
      newProductName: "",
      newProductPrice: "",
      newProductDescription: "",
      newProductImageUrl: "",
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
    createProduct: function() {
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
        this.products.push(response.data)
      });
    },
    updateProduct: function(inputProduct) {
      var params = {
                    name: inputProduct.name,
                    price: inputProduct.price,
                    description: inputProduct.description,
                    image_url: inputProduct.image_url
                    };
      axios.patch( "/api/products/" + inputProduct.id, params )
      .then(response => {
        console.log("Success", response.data);
        inputProduct = response.data;
      });
    },
    destroyProduct: function(inputProduct) {
      axios.delete("/api/products/" + inputProduct.id)
      .then(response => {
        console.log("Success", response.data);
        var index = this.products.indexOf(inputProduct);
        this.products.splice(index,1);
      }); 
    }
  }
};
</script>



