<template>
  <div class="chat-panel">
    <div class="chat-box p-2 d-flex flex-column h-100">      
      <div class="header text-left">
        <span class="title">
          채팅
        </span>
        <button
          class="btn close-btn"
          @click="toggleChatPanel"
        >
          <i class="fas fa-times"></i>
        </button>
      </div>
      <!-- 채팅 내역 -->
      <div
        id="chat-area"
        :height="chatHeight"
      >
        <div 
          class="mt-2 text-left message" 
          v-for="(message, i) of messages" 
          :key="i"
        >
          <div class="message-title">
            <span class="mr-2 message-header">{{ message.sender }}</span>
            <span class="message-header">{{ message.time }}</span>
          </div>
          <div>
            {{ message.message }}
          </div>
        </div>
      </div>
      <div class="footer d-flex mt-auto">
        <div class="col-10 px-1 py-0">
          <input 
            class="text-box"
            v-model="message"
            @keyup.enter="clickSendMessage"
          >
        </div>
        <div class="col-2 p-0">
          <button
            class="send-btn"
            @click="clickSendMessage"
          >
            <i class="fas fa-paper-plane"></i>
          </button>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import { mapState, mapActions } from "vuex";
export default {
  name: "ChatPanel",
  data() {
    return {
      message: "",
      chatHeight: "33vh"
    };
  },
  watch: {
    messages() {
      setTimeout(() => {
        var chatDiv = document.getElementById("chat-area");
        chatDiv.scrollTo({
          top: chatDiv.scrollHeight - chatDiv.clientHeight,
          behavior: 'smooth'
        })
      }, 50);
    },
    currentMode() {
      if (this.currentMode === "") {
        this.chatHeight = "80vh"
      } else {
        this.chatHeight="33vh"
      }
    }
  },
  computed: {
    ...mapState("meetingStore", ["messages", "currentMode"]),
  },
  methods: {
    ...mapActions("meetingStore", [
      "toggleChatPanel",
      "sendMessage",
    ]),
    clickSendMessage() {
      if (this.message.trim()) {
        this.sendMessage(this.message)
        this.message = ""
      }
    }
  },
};
</script>


<style scoped>
.chat-box {
  height: 100%;
}

.header {
  position: relative;
}

.close-btn {
  position: absolute;
  color: white;
  top: 3px;
  right: 10px;
}

.text-box {
  background-color: #D1D1D1;
  width: 100%;
  border-radius: 20px;
  color: black;
  padding-left: 10px;
}

.text-box:focus {
  outline: none;
}

.title {
  padding-left: 5%;
  font-family: 'Jua' !important;
  font-size: 1.0rem !important;
  color: white;
}

.header {
  width: 100%;
  border-radius: 20px;
  box-shadow: 3px 3px 3px rgb(0, 0, 0, 0.3);
  height: 4vh;
}
.message-title {
  
  font-size: 0.8rem;
}

.message-header {
  font-family: 'Nanum Gothic', sans-serif;
}

.message {
  color: white;
}

.send-btn {
  color: white;
}

#chat-area {
  overflow-y: auto;
}

#chat-area::-webkit-scrollbar {
  width: 8px; 
  height: 8px;
}

#chat-area::-webkit-scrollbar-track {
  background: #37474F;
}

#chat-area::-webkit-scrollbar-corner {
  background: #37474F; 
}

#chat-area::-webkit-scrollbar-thumb {
  background:  #b0a2c8;
}

#chat-area::-webkit-scrollbar-button {
  background-color: #37474F;
  height: 0;
}



</style>