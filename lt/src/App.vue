<template>
  <div>
    <h1>VUE API</h1>
    <ul>
      <li v-for="user in users" :key="user.id">
        {{ user.name }}
      </li>
    </ul>
    <button @click="addUser">thêm dữ liệu vào json-server</button>
  </div>
</template>
<script setup>
import { onMounted, ref } from "vue";
// Khai báo hàm lấy dữ liệu từ json-server
const users = ref([]);
const getData = async () => {
  const response = await fetch("http://localhost:3000/users");
  const data = await response.json();
  console.log(data);
  users.value = data;
};
onMounted(() => {
  getData();
});
// khai báo hàm thêm user
const addUser=()=>{
  const newUser={
    name:"Thu Trang"
  }
  fetch("http://localhost:5173/users",{
    method:"POST",
    headers:{
      "Content-Type":"application/json"
    },
    body:JSON.stringify(newUser)
  })

}   
</script>
<style>
</style>