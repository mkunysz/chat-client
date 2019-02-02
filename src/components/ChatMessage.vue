<template>
  <div class="chat-message">
    <strong class="chat-message__user">{{ message.user.name }}: </strong>
    <span class="chat-message__content">{{ message.message }}</span>
  </div>
</template>

<script>
export default {
  name: 'ChatMessage',
  props: {
    message: Object
  }
}
</script>

<style lang="scss">
.chat-message {
  padding: 0 10px;
  text-align: left;
}
</style>
