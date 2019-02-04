<template>
  <div class="chat-conversation">
    <p class="chat-conversation__header">conversation</p>
    <div
      class="chat-conversation__message"
      v-for="message in messages"
      :message="message"
      v-bind:key="message.id">
      <strong class="chat-message__user">{{ message.user.name }}: </strong>
      <span class="chat-message__content">{{ message.message }}</span>
    </div>
  </div>
</template>

<script>
export default {
  name: 'ChatConversation',
  props: {
    socket: {
      type: Object,
      required: true
    }
  },
  data() {
    return {
      messages: []
    };
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
  &__message {
    padding: 0 10px;
    text-align: left;
  }
  &__header {
    border-bottom: 1px solid #ccc;
    margin-bottom: 10px;
  }
}
</style>
