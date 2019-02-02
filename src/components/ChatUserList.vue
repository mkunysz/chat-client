<template>
  <div class="chat-user-list">
    <p>chat-user-list</p>
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
      users: []
    };
  },
  mounted() {
    this.socket.on('connected', (user) => {
      this.users.push(user);
    });
    this.socket.on('disconnected', (user) => {
      for(let i = 0; i < this.users.length; i++) {
        if (user.name === this.users[i].name){
          this.users.splice(i, 1);
          return;
        }
      }
    });
  }
}
</script>

<style lang="scss">
.chat-user-list {
  padding: 0 10px;
  &__user {
    text-align: left;
  }
}
</style>
