<template>
  <div id="app">
    <Container>
      <ChatWindow v-on:sendMessage="sendMessage">
        <ChatMessage v-for="message in messages" :key="message" v-bind:username="message.author" v-bind:datetime="message.datetime" v-bind:text="message.text"></ChatMessage>
      </ChatWindow>
    </Container>
  </div>
</template>
<script>
import Container from "./components/MyContainer.vue";
import ChatMessage from "./components/ChatMessage.vue";
import ChatWindow from "./components/ChatWindow.vue";

export default {
  name: "App",
  components: {
    Container, ChatMessage, ChatWindow
  },

  data() {
    return{
      messages: []
    }
  },

  methods: {
    GetMessages: function(){
      this.axios.get('https://61bcd385d8542f0017824a2a.mockapi.io/messages')
        .then((response) => {
          for(let i in response.data){
            this.messages.push(response.data[i]);
          }
        });
    },

    sendMessage: function(message){
      this.axios.post("https://61bcd385d8542f0017824a2a.mockapi.io/messages?author=" + message.username + "&text=" + message.text /*{"author": message.username,"text": message.text}*/)
        .then(() => {
          this.GetMessages();
        })
        .catch(error => {
          console.log(error.response)
      });
    }
  },

  mounted(){
    this.GetMessages();
  }
};
</script>
<style>
body {
  margin: 0;
  background-color: #f9f9fa;
}
</style>
