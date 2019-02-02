<template>
  <div class="chat-user-list">
    <p class="chat-user-list__header">user list</p>
    <div class="chat-user-list__user" v-for="user in users" v-bind:key="user.name">
      {{ user.name }}
    </div>
  </div>
</template>

<script>
export default {
  name: 'chat-user-list',
  props: {
    socket: Object
  },
  data() {
    return {
      users: {}
    };
  },
  mounted() {
    this.socket.on('users', (users) => {
      this.users = users;
    });
    this.socket.on('disconnect', () => {
      this.users = {};
    });
  }
}
</script>

<style lang="scss">
.chat-user-list {
  &__header {
    border-bottom: 1px solid #ccc;
    margin-bottom: 10px;
  }
  &__user {
    padding: 0 10px;
    text-align: left;
  }
}
</style>
