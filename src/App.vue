<template>
  <div id="app">
    crot
    <pre>
      {{ res }}
    </pre>
    <button v-on:click="res.Connect()">Connect</button>
    <button v-on:click="res.sendMessage('CLICK')">Send Message</button>
    <button v-on:click="res.Close()">Close</button>
  </div>
</template>

<script>
import { ref, reactive, onMounted } from 'vue';

export default {
  name: 'App',
  setup() {
    let connection = new WebSocket('wss://rootenvsocket.aziza.workers.dev/ws');

    const res = reactive({
      data: {},
      Connect: () => {
        connection = new WebSocket('wss://rootenvsocket.aziza.workers.dev/ws');
      },
      sendMessage: () => {
        connection.send('asas');
      },
      Close: () => {
        connection.close();
      },
    });

    onMounted(() => {
      connection.onmessage = (event) => {
        // Vue data binding means you don't need any extra work to
        // update your UI. Just set the `time` and Vue will automatically
        // update the `<h2>`.
        res.data = JSON.parse(event.data);
        setTimeout(() => {
          res.sendMessage();
        }, 2000);
      };

      connection.onopen = function (event) {
        console.log(event);
      };
    });

    return { res };
  },
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
