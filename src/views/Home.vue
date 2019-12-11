<template>
  <div class="home">
    <h1>{{ message }}</h1>
    <div v-for="product in products">
      <h2>{{ product.name }}</h2>
      <p>{{ product.description }}</p>
      <img :src="product.image_url" alt="" />
      <div>
        <button>Buy now!</button>
      </div>
    </div>
    <h2>New product</h2>
    Name
    <input v-model="params.name" type="text" />
    Price
    <input v-model="params.price" type="text" />
    Description
    <input v-model="params.description" type="text" />
    <div>
      <button v-on:click="createProduct">Create</button>
    </div>
  </div>
</template>

<style>
body {
  background-color: white;
}
img {
  width: 300px;
}
</style>

<script>
import axios from "axios";
export default {
  data: function() {
    return {
      message: "Welcome to Vue.js!",
      isActive: true,
      products: [],
      params: {
        name: "",
        price: null,
        description: "",
        image_url: ""
      }
    };
  },
  created: function() {
    axios.get("/api/products").then(response => {
      this.products = response.data;
      console.table(this.products);
    });
  },
  methods: {
    createProduct: function() {
      axios
        .post("/api/products", this.params)
        .then(response => {
          var product = response.data;
          this.products.push(product);
          this.params = {
            name: "",
            price: null,
            description: "",
            image_url: ""
          };
        })
        .catch(error => console.log(error.response));
    }
  }
};
</script>
