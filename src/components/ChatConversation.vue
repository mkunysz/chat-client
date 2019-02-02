<template>
  <div class="chat-conversation">
    <p class="chat-conversation__header">conversation</p>
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
    this.socket.on('disconnect', () => {
      this.messages = [];
    });
  }
}
</script>

<style lang="scss">
.chat-conversation {
  &__header {
    border-bottom: 1px solid #ccc;
    margin-bottom: 10px;
  }
}
</style>
