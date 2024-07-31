<template>
    <div class="title">
        A messaging app
    </div>
    <div class="messaging-app">
      <div class="chat-window">
        <div v-for="message in messages" :key="message.id" class="message">
            <div class="details">
                
                <span class="username">{{ message.username }}</span>
                <span class="timestamp">{{ formatTimestamp(message.timestamp) }}</span>
            </div>
            <span class="text">{{ message.text }}</span>
        </div>
      </div>
      <div class="input-area">
        <input
          type="text"
          v-model="newMessage"
          placeholder="Type a message..."
          @keydown.enter="sendMessage"
        />
        <button @click="sendMessage" :disabled="newMessage.trim() === ''">Send</button>
      </div>
    </div>
  </template>
  
  <script lang="ts">
  import { defineComponent, ref } from 'vue';
  
  interface Message {
    id: number;
    text: string;
    timestamp: Date;
    username: string;
  }
  
  export default defineComponent({
    name: 'Home',
    setup() {
      // Reactive references for messages and new message input
      const messages = ref<Message[]>([
        { id: 1, text: 'Hello!', timestamp: new Date(),username:'user1' },
        { id: 2, text: 'How are you?', timestamp: new Date(),username:'user1' }
      ]);
  
      const newMessage = ref<string>('');
  
      // Function to send a message
      const sendMessage = () => {
        if (newMessage.value.trim() !== '') {
          const newMessageObj: Message = {
            id: messages.value.length + 1,
            text: newMessage.value,
            timestamp: new Date(),
            username:'user1'
          };
          messages.value.push(newMessageObj);
          newMessage.value = '';
        }
      };
  
      // Function to format timestamps for display
      const formatTimestamp = (timestamp: Date): string => {
        return timestamp.toLocaleTimeString([], { hour: '2-digit', minute: '2-digit' });
      };
  
      return {
        messages,
        newMessage,
        sendMessage,
        formatTimestamp
      };
    }
  });
  </script>
  
  <style scoped>
  .messaging-app {
    max-width: 600px;
    margin: 50px auto;
    border: 1px solid #ccc;
    border-radius: 10px;
    overflow: hidden;
    display: flex;
    flex-direction: column;
    background-color: #fff;
  }
  
  .chat-window {
    flex: 1;
    padding: 20px;
    overflow-y: auto;
    border-bottom: 1px solid #ccc;
    background-color: #f9f9f9;
  }
  
  .message {
    display: flex;
    align-items: center;
    margin-bottom: 10px;
  }
  
  .timestamp {
    margin-right: 10px;
    font-size: 12px;
    color: #888;
  }
  
  .text {
    background-color: #e1f5fe;
    padding: 10px;
    border-radius: 8px;
    max-width: 70%;
  }
  
  .input-area {
    display: flex;
    padding: 10px;
    background-color: #fff;
  }
  
  input[type="text"] {
    flex: 1;
    padding: 10px;
    font-size: 16px;
    border: 1px solid #ccc;
    border-radius: 5px;
    margin-right: 10px;
    outline: none;
  }
  
  button {
    padding: 10px 20px;
    background-color: #4a90e2;
    color: white;
    font-size: 16px;
    border: none;
    border-radius: 5px;
    cursor: pointer;
    transition: background-color 0.3s ease;
  }
  
  button:hover {
    background-color: #357abd;
  }
  
  button:disabled {
    background-color: #ccc;
    cursor: not-allowed;
  }
  .details{
    display: flex;
    flex-direction: column;
  }
  </style>