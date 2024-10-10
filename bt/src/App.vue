<template>
  <div>
    <h1>Ex session 22</h1>
    <h2>Add Product </h2>
    <button @click="showProductModal" class="btn-open-modal">Add Product</button>
  
    <!-- Modal -->
    <div v-if="isModalVisible" class="modal-overlay">
      <div class="modal-container">
        <span class="modal-close" @click="hideProductModal">&times;</span>
        <h2>Add New Product</h2>
  
        <form @submit.prevent="submitProductForm">
          <div class="form-field">
            <label for="name">Product Name</label>
            <input type="text" v-model="productForm.name" id="name" required />
          </div>
  
          <div class="form-field">
            <label for="image">Image URL</label>
            <input type="text" v-model="productForm.image" id="image" required />
          </div>
  
          <div class="form-field">
            <label for="price">Price</label>
            <input type="number" v-model="productForm.price" id="price" required />
          </div>
  
          <div class="form-field">
            <label for="quantity">Quantity</label>
            <input type="number" v-model="productForm.quantity" id="quantity" required />
          </div>
  
          <button type="submit" class="btn-submit">Add Product</button>
        </form>
      </div>
    </div>
  
    <div class="product-table">
      <div class="table-header">
        <div class="table-cell">#</div>
        <div class="table-cell">Product Name</div>
        <div class="table-cell">Image</div>
        <div class="table-cell">Price</div>
        <div class="table-cell">Quantity</div>
        <div class="table-cell">Actions</div>
      </div>
      <div class="table-row" v-for="(product, index) in products" :key="product.id">
        <div class="table-cell">{{ index + 1 }}</div>
        <div class="table-cell">{{ product.name }}</div>
        <div class="table-cell">
          <img :src="product.image" alt="Product Image" />
        </div>
        <div class="table-cell">{{ product.price }}$</div>
        <div class="table-cell">{{ product.quantity }}</div>
        <div class="table-cell">
          <button @click="showProductDetails(product)">Details</button>
          <button @click="deleteProduct(product.id)">Delete</button>
        </div>
      </div>
    </div>
  </div>
  </template>
  
  <script setup>
  import { ref, onMounted } from 'vue';
  
  const products = ref([]);
  const isModalVisible = ref(false);
  const productForm = ref({
    id: Math.ceil(Math.random() * 1000),
    name: '',
    image: '',
    price: null,
    quantity: null,
  });
  
  const fetchProducts = async () => {
    const response = await fetch("http://localhost:3000/products");
    const data = await response.json();
    products.value = data;
  };
  
  onMounted(fetchProducts);
  
  const showProductDetails = (item) => {
    console.log("Product Details:", "Name:", item.name, "---", "Price:", item.price, "---", "Quantity:", item.quantity);
  };
  
  const deleteProduct = async (id) => {
    await fetch(`http://localhost:3000/products/${id}`, {
      method: "DELETE",
    });
    fetchProducts();
  };
  
  const showProductModal = () => {
    isModalVisible.value = true;
  };
  
  const hideProductModal = () => {
    isModalVisible.value = false;
  };
  
  const submitProductForm = async () => {
    const newProduct = productForm.value;
    await fetch("http://localhost:3000/products", {
      method: "POST",
      headers: {
        "Content-Type": "application/json",
      },
      body: JSON.stringify(newProduct),
    });
    productForm.value = { id: Math.ceil(Math.random() * 1000), name: '', image: '', price: null, quantity: null };
    hideProductModal();
    fetchProducts();
  };
  </script>
  
  <style scoped>
  .product-table {
    display: table;
    width: 700px;
    border-collapse: collapse;
    margin-top: 20px;
    font-family: Arial, sans-serif;
  }
  
  .table-header {
    display: table-row;
    background-color: #4CAF50;
    color: white;
    font-weight: bold;
    text-transform: uppercase;
  }
  
  .table-row {
    display: table-row;
    background-color: #f9f9f9;
  }
  
  .table-row:nth-child(odd) {
    background-color: #f1f1f1;
  }
  
  .table-cell {
    display: table-cell;
    padding: 5px;
    border: 1px solid #ccc;
    text-align: center;
    vertical-align: middle;
  }
  
  .table-cell img {
    max-width: 150px;
    height: auto;
    border-radius: 8px;
  }
  
  .modal-overlay {
    display: flex;
    justify-content: center;
    align-items: center;
    position: fixed;
    z-index: 1;
    left: 0;
    top: 0;
    width: 100%;
    height: 100%;
    background-color: rgba(0, 0, 0, 0.5);
  }
  
  .modal-container {
    background-color: white;
    padding: 20px;
    border-radius: 8px;
    width: 400px;
    position: relative;
  }
  
  .modal-close {
    position: absolute;
    top: 10px;
    right: 15px;
    font-size: 24px;
    cursor: pointer;
  }
  
  .form-field {
    margin-bottom: 15px;
  }
  
  .form-field label {
    display: block;
    margin-bottom: 5px;
  }
  
  .form-field input {
    width: 100%;
    padding: 8px;
    border: 1px solid #ccc;
    border-radius: 4px;
  }
  
  .btn-submit {
    background-color: #4CAF50;
    color: white;
    padding: 10px 15px;
    border: none;
    border-radius: 4px;
    cursor: pointer;
  }
  
  .btn-submit:hover {
    background-color: #45a049;
  }
  
  .btn-open-modal {
    background-color: #008CBA;
    color: white;
    padding: 10px 15px;
    border: none;
    border-radius: 4px;
    cursor: pointer;
  }
  
  .btn-open-modal:hover {
    background-color: #007bb5;
  }
  </style>
  