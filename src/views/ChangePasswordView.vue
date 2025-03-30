<script setup lang="ts">
import { ref } from "vue";
import axios from "axios";

const username = ref<string>("");
const email = ref<string>("");
const new_password = ref<string>("");
const status = ref<string>("");

const handleSubmit = async (event: Event) => {
  event.preventDefault();
  console.log("handling submit...");

  try {
    const response = await axios.post(
      "http://localhost:8000/auth/change-password/",
      {
        username: username.value,
        email: email.value,
        new_password: new_password.value,
      }
    );

    if (response.status === 200) {
      status.value = "Password changed successful.";
    } else {
      status.value = "Invalid credentials.";
    }

    username.value = "";
    email.value = "";
    new_password.value = "";
  } catch (e) {
    console.error(e);
  }
};
</script>

<template>
  <h1>change password</h1>

  <form>
    <input v-model="username" type="text" placeholder="username" />
    <input v-model="email" type="email" placeholder="e-mail" />
    <input
      v-model="new_password"
      type="password"
      placeholder="repeated password"
    />
    <span>{{ status }}</span>
    <button @click="handleSubmit">change</button>
  </form>
</template>
