<script setup lang="ts">
import { ref } from "vue";
const message = ref<string>("");
const chatHistory = ref<string[]>(["Das ist eine vor definierte Nachricht"]);

const handleClick = () => {
  console.log("click handled");
  chatHistory.value.push(message.value);
};
</script>

<template>
  <div class="chat">
    <div id="ChatHistory">
      <span>Chat history</span>

      <p></p>

      <div v-for="message in chatHistory" :key="message">
        <span>{{ message }}</span>
      </div>

      <p></p>
    </div>
    <div>
      <input
        v-model="message"
        type="text"
        placeholder="enter your message..."
      />
      <button @click="handleClick">send</button>
    </div>
  </div>
</template>

<style scoped>
.chat {
  width: 500px;
}
</style>
