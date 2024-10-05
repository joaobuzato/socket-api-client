<template>
  <input v-model="message" @keyup.enter="sendMessage" />
</template>

<script>
import { ref, computed, watch, onMounted, onUnmounted } from "vue";
import { io } from "socket.io-client";

const socket = io("http://localhost:3000", { path: "/chat" });

export default {
  name: "Input",
  props: {},
  setup(_, { emit }) {
    // Data
    //this is a reactive variable
    //it will trigger a re-render when it changes
    // it is defined as a ref because it is a primitive type
    //it is defined as a v-model on the component input.
    const message = ref("");

    // Computed
    // const computedExample = computed(
    //   () => `${props.propExample} - ${dataExample.value}`
    // );

    // Methods
    const sendMessage = () => {
      //this emit will send the message to the parent component
      socket.emit("chat message", message.value);
      message.value = "";
    };

    // Watch
    watch(message, (newValue, oldValue) => {
      console.log("dataExample changed from", oldValue, "to", newValue);
    });

    // Lifecycle hooks
    onMounted(() => {
      console.log("Component mounted");
    });

    onUnmounted(() => {
      console.log("Component unmounted");
    });

    return {
      message,
      sendMessage,
    };
  },
};
</script>

<style scoped>
/* Add your styles here */
</style>
