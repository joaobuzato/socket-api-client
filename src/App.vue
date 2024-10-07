<script>
import Input from "./components/Input.vue";
import MessageList from "./components/MessageList.vue";

export default {
  components: {
    Input,
    MessageList,
  },
  data() {
    return {
      messages: [],
      username: "",
      color: "#000000",
      isLoading: true,
    };
  },
  methods: {
    handleMessageSent(message) {
      this.messages.push({
        username: this.username,
        color: this.color,
        message,
      });
    },
  },

  created() {
    fetch("http://localhost:3000/messages")
      .then((response) => response.json())
      .then((data) => {
        this.messages = data;
        console.log("Messages loaded:", this.messages);
        this.isLoading = false;
      })
      .catch((error) => {
        console.error("Failed to load messages:", error);
      });
  },
};
</script>

<style scoped>
.container {
  display: flex;
  flex-direction: column;
  align-items: center;
}

.message-list {
  flex-grow: 1;
  overflow-y: auto;
  height: 85vh;
  width: 80%;
  background-color: #f8f8f8;
}

.input-area {
  display: flex;
  padding: 16px;
  height: 10vh;
  width: 80%;
  background-color: #424242;
}

.username-input,
.message-input,
.color-picker {
  margin-right: 10px;
}

.username-input,
.color-picker {
  flex-grow: 0.1;
  height: 100%;
  place-self: center;
}

.message-input {
  flex-grow: 1;
}
</style>

<template>
  <div class="container">
    <div v-if="isLoading" class="message-list">Loading messages...</div>
    <MessageList v-else :messages="messages" class="message-list" />
    <div class="input-area">
      <input
        type="text"
        v-model="username"
        placeholder="Enter username"
        class="username-input"
      />
      <Input
        @message-sent="handleMessageSent"
        :color="this.color"
        :username="this.username"
        class="message-input"
      />
      <input type="color" v-model="color" class="color-picker" />
    </div>
  </div>
</template>
