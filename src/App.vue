<template>
  <div class="chat">
    <div class="chat__form-wrapper" v-if="!user.name">
      <h1>Welcome to Vuejs Chat</h1>
      <div class="chat__form">
        <input class="chat__name-input" v-model="preselectedName" placeholder="Select your name" />
        <button class="chat__save-button" @click="saveName">Enter Chat</button>
      </div>
    </div>
    <div class="chat__wrapper" v-else>
      <div class="chat__conversation">
        <chat-conversation :socket="socket">
        </chat-conversation>
        <div class="chat__controls">
          <input @keyup="onKeyUp" class="chat__message-input" v-model="message" />
          <button class="chat__send-button" @click="sendMessage">Send</button>
        </div>
      </div>
      <div class="chat__participants">
        <chat-user-list :socket="socket">
        </chat-user-list>
      </div>
    </div>
  </div>
</template>

<script>
import io from 'socket.io-client';
import ChatConversation from './components/ChatConversation.vue';
import ChatUserList from './components/ChatUserList.vue';

export default {
  name: 'app',
  components: {
    ChatConversation,
    ChatUserList
  },
  data() {
    return {
      message: '',
      socket: null,
      user: {
        name: ''
      },
      preselectedName: ''
    };
  },
  beforeDestroy() {
    if (this.socket) {
      this.socket.emit('disconnected', {
        user: this.user
      });
    }
  },
  methods: {
    connect() {
      this.socket.emit('connected', this.user);
    },
    sendMessage() {
      this.socket.emit('message', {
        user: this.user,
        message: this.message
      });
      this.message = '';
    },
    onKeyUp(event) {
      if (event.keyCode === 13) {
        this.sendMessage();
      }
    },
    saveName() {
      this.user.name = this.preselectedName;
      this.preselectedName = '';
      this.socket = io('http://localhost:8081');
      this.socket.on('connect', () => {
        this.connect();
      });
    }
  }
}
</script>

<style lang="scss">
html {
  box-sizing: border-box;
  font-size: 16px;
}

*, *:before, *:after {
  box-sizing: inherit;
}

html,body {
  height: 100%;
}

body, h1, h2, h3, h4, h5, h6, p, ol, ul {
  margin: 0;
  padding: 0;
  font-weight: normal;
}

ol, ul {
  list-style: none;
}

img {
  max-width: 100%;
  height: auto;
}
.chat {
  height: 100%;
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin: 20px;
  &__wrapper {
    display: flex;
    flex-direction: row;
    flex: 1;
    border: 1px solid #ccc;
  }
  &__conversation {
    flex: 4;
    display: flex;
    flex-direction: column;
    height: 500px;
  }
  &__participants {
    flex: 1;
    height: 500px;
    border-left: 1px solid #ccc;
  }
  &__controls {
    height: 30px;
    display: flex;
    margin-top: auto;
  }
  &__message-input {
    height: 30px;
    padding: 0 10px;
    flex: 12;
    border: none;
    border-top: 1px solid #ccc;
    border-right: 1px solid #ccc;
    outline: none;
  }
  &__send-button {
    height: 30px;
    flex: 1;
    border: 0;
    background: white;
    border-top: 1px solid #ccc
  }
  &__form-wrapper {
    height: 100%;
    display: flex;
    align-items: center;
    justify-content: center;
    flex-direction: column;
  }
  &__form {
    margin: 10px 0;
  }
  &__name-input,
  &__save-button {
    height: 30px;
    border: 1px solid #ccc;
    margin: 0 5px;
    padding: 0 15px;
    outline: none;
  }
}
</style>
