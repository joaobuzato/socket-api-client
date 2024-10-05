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
    };
  },
  methods: {
    handleMessageSent(message) {
      this.messages.push(message);
    },
  },
};
</script>

<template>
  <div>
    <MessageList :messages="messages" />
    <Input @message-sent="handleMessageSent" />
  </div>
</template>
