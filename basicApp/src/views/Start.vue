<template>
  
    <div class="app-container">
      <div class="title"> Messaging App </div>
      <button class="start-button" @click="navigateToLogin">Start</button>
      
    </div>
  </template>
  
  <script lang="ts">
    import { defineComponent } from 'vue';
    import { useRouter } from 'vue-router';

    export default defineComponent({
    name: 'Home',
    setup() {
        const router = useRouter();
        
        const navigateToLogin = () => {
        router.push({ name: 'Login' });
        };

        return {
        navigateToLogin
        };
    }
    });
  </script>
  
  <style>
  
  /* Set up the gradient background */
  .app-container {
    height: 100vh; /* Full viewport height */
    width:100vw;
    margin:0;
    display: flex;
    justify-content: center;
    align-items: center;
    gap:10%;
    flex-direction: column;
    background: linear-gradient(135deg, #ff7e5f, #feb47b);
    padding: 0;
  }
  
  /* Style the start button */
  .start-button {
    padding: 15px 30px;
    font-size: 18px;
    color: white;
    background-color: #ff6f61;
    border: none;
    border-radius: 8px;
    cursor: pointer;
    transition: background-color 0.3s ease;
  }
  
  .start-button:hover {
    background-color: #e65c50;
  }
  .title{
    color:white;
    font-family: "Poppins", sans-serif;
    font-weight: 600;
    font-style: normal;
    font-size:40px;
  
  }
  </style>