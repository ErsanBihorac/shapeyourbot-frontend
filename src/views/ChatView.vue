<script setup lang="ts">
import { onMounted, ref } from "vue";
const message = ref<string>("");
const chatHistory = ref<string[]>(["Das ist eine vor definierte Nachricht"]);
const webSocket = ref<WebSocket | null>(null);

const initWebsocketConnection = () => {
  webSocket.value = new WebSocket(`ws://localhost:8000/ws/socket-server/`);

  webSocket.value.onmessage = function (e) {
    let data = JSON.parse(e.data);
    console.log("Data: ", data);

    if (data.type === "chat") {
      chatHistory.value.push(data.message);
    }
  };

  webSocket.value.onerror = function (error) {
    console.error("WebSocket Error: ", error);
  };

  webSocket.value.onclose = function () {
    console.log("WebSocket connection closed.");
  };
};

const handleClick = () => {
  if (webSocket.value && message.value.trim() !== "") {
    webSocket.value.send(
      JSON.stringify({
        message: `You: ${message.value}`,
      })
    );
    message.value = ""; 
  }
};

onMounted(() => {
  initWebsocketConnection();
});
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
