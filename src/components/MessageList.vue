<template>
  <div>
    <div v-for="(msg, index) in messages" :key="index">
      {{ msg }}
    </div>
  </div>
</template>

<script>
import { ref, onMounted, onUnmounted } from "vue";
import { io } from "socket.io-client";

const socket = io("http://localhost:3000", { path: "/chat" });

export default {
  name: "MessageList",
  props: {
    messages: {
      type: Array,
      required: true,
    },
  },
  setup() {
    const messages = ref([]);

    const addMessage = (msg) => {
      messages.value.push(msg);
    };

    onMounted(() => {
      socket.on("chat message", addMessage);
    });

    onUnmounted(() => {
      socket.off("chat message", addMessage);
    });

    return { messages };
  },
};
</script>
