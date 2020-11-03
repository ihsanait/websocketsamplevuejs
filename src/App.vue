<template>
  <div id="app">
    <h2>Vue.js WebSocket</h2>
    <h5 v-text="getData"></h5>
    <input v-model="message" />
    <button v-on:click="sendMessage()">Send Message</button>
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
    };
  },
  created() {
    this.connection = new WebSocket("ws://localhost:5000/testws");
    this.connection.onmessage = function (event) {
      console.log(this);
      console.log(event);
      console.log(event.data);
    };
  },
  computed: {
    getData() {
      return this.eventData;
    },
  },
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
