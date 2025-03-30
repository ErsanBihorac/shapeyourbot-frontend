<script setup lang="ts">
import { ref } from "vue";
import axios from "axios";

const username = ref<string>("");
const password = ref<string>("");
const status = ref<string>("");

const handleSubmit = async (event: Event) => {
  event.preventDefault();
  console.log("handling submit...");

  try {
    const response = await axios.post("http://localhost:8000/auth/login/", {
      username: username.value,
      password: password.value,
    });

    if (response.status === 200) {
      status.value = "login was successful.";
    } else {
      status.value = "Invalid login credentials.";
    }

    username.value = "";
    password.value = "";
  } catch (e) {
    console.error(e);
  }
};
</script>

<template>
  <h1>login</h1>

  <form>
    <input v-model="username" type="text" placeholder="username" />
    <input v-model="password" type="password" placeholder="password" />
    <span>{{ status }}</span>
    <button @click="handleSubmit">login</button>
  </form>
</template>
