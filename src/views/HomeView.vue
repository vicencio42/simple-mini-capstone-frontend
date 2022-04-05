<template>
  <div class="home">
    <h1 v-bind:class="message">{{ message }} count: {{ products.length }}</h1>
    <h2 class="greeting">{{ greeting }}</h2>
    Name
    <input type="text" v-model="productName" />
    Price
    <input type="text" v-model="productPrice" />
    Description
    <input type="text" v-model="productDescription" />
    Image URL
    <input type="text" v-model="productImgUrl" />
    <button v-on:click="createProduct">Create Stuff</button>
    <div v-for="product in products" v-bind:key="product.id">
      <h3>{{ product.name }}</h3>
      <p class="product">{{ product.price }}</p>
      <img v-bind:src="product.image_url" v-bind:alt="product.name" />
    </div>
  </div>
</template>

<script>
import axios from "axios";

export default {
  data: function () {
    return {
      message: "Welcome to Vue.js By Vin!",
      greeting: "Let's write some vue code!",
      products: [],
      productName: "",
      productDescription: "",
      productPrice: "",
      productImgUrl: "",
    };
  },
  created: function () {
    this.indexProducts();
  },
  methods: {
    indexProducts() {
      axios.get("http://localhost:3000/products").then((response) => {
        console.log(response.data);
        this.products = response.data;
      });
    },
    createProduct() {
      var params = {
        name: this.productName,
        price: this.productPrice,
        description: this.productDescription,
        image_url: this.productImgUrl,
      };
      axios
        .post("http://localhost:3000/products", params)
        .then((response) => {
          this.products.push(response.data);
        })
        .catch((error) => {
          console.log(error.response);
        });
    },
  },
};
</script>

<style>
.message {
  color: blue;
  font-weight: bold;
}
.greeting {
  color: red;
  font-style: italic;
}
.product {
  color: rgb(61, 208, 56);
  font-style: italic;
  font-weight: bold;
}
</style>
