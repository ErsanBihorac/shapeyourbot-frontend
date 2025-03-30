<script setup lang="ts">
import { ref } from "vue";
import axios from "axios";

const username = ref<string>("");
const email = ref<string>("");
const password = ref<string>("");
const repeated_password = ref<string>("");
const status = ref<string>("");

const handleSubmit = async (event: Event) => {
  event.preventDefault();
  console.log("handling submit...");

  try {
    const response = await axios.post(
      "http://localhost:8000/auth/registration/",
      {
        username: username.value,
        email: email.value,
        password: password.value,
        repeated_password: repeated_password.value,
      }
    );

    if (response.status === 200) {
      status.value = "Registration was successful.";
    } else {
      status.value = "Invalid registration credentials.";
    }

    username.value = "";
    email.value = "";
    password.value = "";
    repeated_password.value = "";
  } catch (e) {
    console.error(e);
  }
};
</script>

<template>
  <h1>register</h1>

  <form>
    <input v-model="username" type="text" placeholder="username" />
    <input v-model="email" type="email" placeholder="e-mail" />
    <input v-model="password" type="password" placeholder="password" />
    <input
      v-model="repeated_password"
      type="password"
      placeholder="repeated password"
    />
    <span>{{ status }}</span>
    <button @click="handleSubmit">register</button>
  </form>
</template>
