<template>
  <div class="container">
    <div class="row mb-3">
      <div class="col-xs-12 col-sm-8 offset-sm-2 col-md-6 offset-md-3 text-left">
        <h3 class="text-center">Vue Resource</h3>
        <div class="form-group">
          <input class="form-control" type="text" v-model="product.name" placeholder="  product.name">
        </div>
        <div class="form-group">
          <input class="form-control" type="number" min="0" v-model="product.price" placeholder="  product.price">
        </div>
        <div class="form-group">
          <input class="form-control" type="text" v-model="product.description" placeholder="  product.description">
        </div>
          <button class="btn btn-block btn-success" @click="submitProduct">Submit</button>
          <button class="btn btn-block btn-success" @click="updateProduct">Update</button>
      </div>
    </div>
    <button class="btn btn-success" @click="getAllProducts">Get All Data</button>
    <table class="table" v-if="product">
      <thead>
        <tr>
          <th scope="col">#</th>
          <th scope="col">Name</th>
          <th scope="col">Price</th>
          <th scope="col">Description</th>
          <th scope="col">Action</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="(item, index) in product" v-bind:key="index">
          <th scope="row">{{ item.id }}</th>
          <td>{{ item.name }}</td>
          <td>{{ item.price }}</td>
          <td>{{ item.description }}</td>
          <td>
            <button class="btn btn-primary" @click="editProduct(item.id)">Edit</button>
            <button class="btn btn-danger" @click.prevent="deleteProduct(item.id)">Delete</button>
          </td>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<script>
import axios from 'axios';
export default {
  name: 'app',
  data() {
    return {
      product: {
        id: 0,
        name: '',
        price: 0,
        description: ''
      }
    }
  },
  methods: {
    submitProduct() {
      // AXIOS
      axios.post("http://localhost:3001/products", {
        name: this.product.name,
        price: this.product.price,
        description: this.product.description
      })
        .then(response => {
          alert("Them moi thanh cong")
          console.log(response);
        }, error => {
          console.log(error);
        })
    },
    getAllProducts() {
      const data = axios.get("http://localhost:3001/products")
        // console.log(data);
        .then(response => {
          this.product = response.data
          console.log(response.data);
        }, error => {
          console.log(error);
        })
    },
    editProduct(id) {
      console.log(id);
      const product = axios.get(`http://localhost:3001/products/${id}`)
        .then(response => {
          this.product = response.data;
          console.log(response.data);
        }, error=>{
          console.log(error);
        })
    },
    updateProduct(){
      const product = axios.put(`http://localhost:3001/products/${this.product.id}`, {
        name: this.product.name,
        price: this.product.price,
        description: this.product.description
      })
      .then(response=>{
        alert('Cap nhat thanh cong')
        console.log(response);
      }, error=>{
        console.log(error);
      })
    },
    deleteProduct(id) {
      console.log(id);
      let confirm = window.confirm("You want to delete the product?");
      if (confirm) {
        const products = axios.delete(`http://localhost:3001/products/${id}`);
        console.log("Product", products);
        alert("Product deleted!");
        // window.location.href(``)
      }
    },
  },
}
</script>

<style>

</style>
