<template>
  <div class="home">
    <div>
      <ul>
        <li v-for="(message, index) in messages" :key="index">{{message}}</li>
      </ul>
    </div>
    <input type="text" v-model="message" />
    <button @click="handleSubmitNewMessage">send</button>
  </div>
</template>

<script>
import io from "socket.io-client"
const url = "http://localhost:3000"
const socket = io(url)
export default {
  name: 'Home',
  data() {
    return {
      message: "",
      messages: []
    }
  },
  methods: {
    handleSubmitNewMessage() {
      socket.emit("message", {data: this.message})
    }
  },
  mounted() {
    socket.on("message", ({data}) => {
      this.messages.push(data)
    })
  }
}
</script>
