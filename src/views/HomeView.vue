<template>
  <div class="home">
    <h1 v-bind:class="message">{{ message }} count: {{ products.length }}</h1>
    <h2 class="greeting">{{ greeting }}</h2>
    Name
    <input type="text" v-model="newProduct.name" />
    Price
    <input type="text" v-model="newProduct.price" />
    Description
    <input type="text" v-model="newProduct.description" />
    Image URL
    <input type="text" v-model="newProduct.image_url" />
    <button v-on:click="createProduct">Create Stuff</button>
    <div v-for="product in products" v-bind:key="product.id">
      <h3>{{ product.name }}</h3>
      <p class="product">{{ product.price }}</p>
      <img v-bind:src="product.image_url" v-bind:alt="product.name" style="max-height: 200px" />
      <div>
        <button v-on:click="showProduct(product)">Product Info</button>
      </div>
    </div>
  </div>
  <dialog id="product-info">
    <form method="dialog">
      <h1>Product Info</h1>
      <p>Name: {{ currentProduct.name }}</p>
      <p>Price: {{ currentProduct.price }}</p>
      <p>Description: {{ currentProduct.description }}</p>
      <p>Image URL: {{ currentProduct.image_url }}</p>
      <h1>Edit Product</h1>
      <p>
        Name:
        <input v-model="currentProduct.name" type="text" />
      </p>
      <p>
        Price:
        <input v-model="currentProduct.price" type="text" />
      </p>
      <p>
        Description:
        <input v-model="currentProduct.description" type="text" />
      </p>
      <p>
        Image URL:
        <input v-model="currentProduct.image_url" type="text" />
      </p>
      <p></p>
      <button v-on:click="updateProduct()">Update</button>
      <button v-on:click="deleteProduct(currentProduct)">Delete</button>
      <button>Close</button>
    </form>
  </dialog>
</template>

<script>
import axios from "axios";

export default {
  data: function () {
    return {
      message: "Welcome to Vue.js By Vin!",
      greeting: "Let's write some vue code!",
      products: [],
      newProduct: {},
      currentProduct: {},
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
      axios.post("http://localhost:3000/products", this.newProduct).then((response) => {
        this.products.push(response.data);
      });
    },
    showProduct(product) {
      axios.get(`http://localhost:3000/products/${product.id}`).then((response) => {
        this.currentProduct = response.data;
        this.editProduct = response.data;
        document.querySelector("#product-info").showModal();
      });
    },
    updateProduct() {
      axios.patch(`http://localhost:3000/products/${this.currentProduct.id}`, this.currentProduct).then((response) => {
        console.log("Success!", response);
        var index = this.products.indexOf(this.currentProduct);
        this.products.splice(index, 1);
        this.products.push(response.data);
      });
    },
    deleteProduct(product) {
      axios.delete(`http://localhost:3000/products/${product.id}`).then((response) => {
        console.log(response);
        var index = this.products.indexOf(product);
        this.products.splice(index, 1);
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
