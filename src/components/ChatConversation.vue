<template>
  <div class="chat-conversation">
    <p>chat-conversation</p>
    <chat-message
      v-for="message in messages"
      :message="message"
      v-bind:key="message.id"
    ></chat-message>
  </div>
</template>

<script>
import ChatMessage from './ChatMessage.vue';

export default {
  name: 'ChatConversation',
  components: {
    ChatMessage
  },
  data() {
    return {
      messages: []
    };
  },
  props: {
    socket: Object
  },
  mounted() {
    this.socket.on('message', (data) => {
      this.messages.push(data);
    });
  }
}
</script>

<style lang="scss">
.chat-conversation {
  padding: 0 10px;
}
</style>
