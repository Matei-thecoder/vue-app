<template>
    <div class="title">
        A messaging app
    </div>
    <div class="subtitle">
      Logged in as <span style="font-weight: bold;">{{ username }}</span>
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
  import axios from 'axios';
//import { timeStamp } from 'console';
import { defineComponent, ref, watch } from 'vue';
  import { useRoute } from 'vue-router'

  const loading = ref(false);
  const post = ref(null);
  const error = ref(null);
  const route = useRoute()
  function getCookieValue(name: string): string | undefined {
    const value = `; ${document.cookie}`;
    const parts = value.split(`; ${name}=`);
    
    if (parts.length === 2) {
      return parts.pop()?.split(";").shift();
    }

    return undefined;
  }
  
  
  
  interface Message {
    id: number;
    text: string;
    timestamp: Date;
    username: string;
  }
  let messagesFromDb : {id:number, text:string, username:string}[];
  let numberOfMessages : number;
  /*const fetchMessages = async()=>{
        await axios.post('https://basicexpress.onrender.com/getMessages')
        .then(res=>{
          if(res.data == "No messages")
          {
              console.log("no messages");
          }
          else
          {
            console.log(res.data);
            for(let i=0;i<res.data.length;i++)
            {
              messagesFromDb = res.data;
            }
            numberOfMessages = res.data.length;
          }
          
          
        })
        .catch(e=>{
          throw(e);
        })
      }
  fetchMessages();*/
  
  
  export default defineComponent({
    name: 'Home',
    setup() {
      // Reactive references for messages and new message input
      const username = getCookieValue("username") || "anonymous";
      const messages = ref<Message[]>([]);
      const fetchMessages = async()=>{
        await axios.post('https://basicexpress.onrender.com/getMessages')
          .then(res=>{
          console.log(res.data);
          messages.value=[];
          for(let i=0;i<res.data.length;i++)
          {
            const newMessageObj: Message = {
                id: res.data[i].id,
                text: res.data[i].text,
                timestamp: new Date(),
                username:res.data[i].username
              };

              messages.value.push(newMessageObj);
          }
          
        })
        .catch(e=>{
          throw(e);
        })
      }
      fetchMessages();
      
      
      
  
      const newMessage = ref<string>('');
  
      // Function to send a message
      /*const sendMessage = () => {
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
      };*/
      
      const sendMessage = async() =>{
        let time = new Date();
        let time_string : string = time.toLocaleString();
        let data = {
          text: newMessage.value,
          time: time_string,
          username:username
        }
        await axios.post("https://basicexpress.onrender.com/sendMessage",data)
        .then(res=>{
          console.log(res.data);
          if(res.data == "ok")
          {
            newMessage.value ='';
            fetchMessages();
          }
          else
          {
            console.log(res.data);
          }
          
        })
        .catch(e=>{throw(e)})
      }
  
      // Function to format timestamps for display
      const formatTimestamp = (timestamp: Date): string => {
        return timestamp.toLocaleTimeString([], { hour: '2-digit', minute: '2-digit' });
      };
     
      return {
        messages,
        newMessage,
        sendMessage,
        formatTimestamp,
        username
      };
    }
   
  });
  </script>
  
  <style scoped>
 
  .messaging-app {
    height:40vh;
    max-width: 600px;
    margin: 50px auto;
    border: 1px solid #ccc;
    border-radius: 20px;
    
    display: flex;
    flex-direction: column;
    background-color: #fff;
    border-radius:20px;
  }
 
  .subtitle{
    color:white;
    width:100%;
    text-align: center;
    font-size: 20px;
  }
  
  .chat-window {
    height:100px;
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