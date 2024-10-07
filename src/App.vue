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
      .then((res) => res.json())
      .then((data) => {
        this.messages = data;
      });
  },
};
</script>

<template>
  <div>
    <input type="text" v-model="username" placeholder="Enter username" />
    <input type="color" v-model="color" />
    <MessageList :messages="messages" />
    <Input
      @message-sent="handleMessageSent"
      :color="this.color"
      :username="this.username"
    />
  </div>
</template>
