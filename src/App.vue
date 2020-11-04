<template>
  <div id="app">
    <h2>Vue.js WebSocket</h2>
    <h4>{{ systemMessage }}</h4>
    <input v-model="message" />
    <button v-on:click="sendMessage()">Send Message</button>
    <li v-for="item in receivedMessages" :key="item">
      {{ item }}
    </li>
  </div>
</template>

<script>
export default {
  name: "App",
  data() {
    return {
      connection: null,
      eventData: "DATA",
      message: "",
      receivedMessages: [],
      systemMessage: "",
    };
  },
  created() {
    this.connection = new WebSocket("ws://192.168.2.6:5000/ws");
    this.connection.onmessage = function (event) {
      // if received message is not system message
      if (event.data.substring(0, 4) != "sys:") {
        this.receivedMessages.push(event.data);
      } else this.systemMessage = event.data;
    }.bind(this);
  },
  computed: {},
  methods: {
    sendMessage() {
      this.connection.send(this.message);
    },
  },
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
