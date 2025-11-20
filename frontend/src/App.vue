<script setup>
  import api from "./services/api";
  import { ref, onMounted } from "vue";

  const users = ref([]);
  const name = ref("");

  onMounted(async () => {
    users.value = (await api.get("/users")).data;
  });

  async function addUser() {
    await api.post("/users", { name: name.value });
    users.value = (await api.get("/users")).data;
    name.value = "";
  }
</script>

<template>
  <h1>User List</h1>

  <input v-model="name" placeholder="Name">
  <button @click="addUser">Add</button>

  <ul>
    <li v-for="u in users" :key="u.id">
      {{ u.name }}
    </li>
  </ul>
</template>
