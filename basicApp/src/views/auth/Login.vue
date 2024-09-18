<template>
    <div class="login-container">
      <h1>Login</h1>
      <form @submit.prevent="handleSubmit">
        <div class="form-group">
          <label for="username">Username:</label>
          <input
            type="text"
            id="username"
            v-model="username"
            placeholder="Enter your username"
            required
          />
        </div>
  
        <div class="form-group">
          <label for="password">Password:</label>
          <input
            type="password"
            id="password"
            v-model="password"
            placeholder="Enter your password"
            required
          />
        </div>
  
        <button type="submit" class="login-button">Login</button>
        <p color="black">If you dont have an account,<a href="./signup">signup</a>.</p>
      </form>
    </div>
  </template>
  
  <script lang="ts">
  import { defineComponent, ref } from 'vue';
  import axios from 'axios';
import router from '@/router';
  function setCookie( id: string, value: string) {

    document.cookie = id + '=' + value;

  }
  export default defineComponent({
    name: 'Login',
    setup() {
      // Define reactive references for username and password
      const username = ref<string>('');
      const password = ref<string>('');
        
      // Handle form submission
      const handleSubmit = () => {
        // Simple validation for demonstration
        if (username.value.trim() === '' || password.value.trim() === '') {
          alert('Please fill in both fields.');
          return;
        }
  
        // Simulate a login process
        alert(`Logging in with username: ${username.value} and password: ${password.value}`);
        let data = {
          user:username.value,
          password:password.value
        }
        axios.post('https://basicexpress.onrender.com/login',data)
        .then(res=>{
          if(res.data.result == 'ok')
          {
              console.log('ok');
              //setCookie('username',`${username.value}`);
              setCookie('username',res.data.username);
              router.push('home');
          }
          else if(res.data == 'error1')
          {
            console.log('user doesnt exist');
            alert('user doesnt exist');
          }
          else if( res.data == 'error2')
          {
            console.log('incorrect password');
            alert('incorrect password');
          }
        })
        .catch(e=>{
          console.log(e);
          alert('error');
        })
        
        // Reset the form
        username.value = '';
        password.value = '';
      };
  
      return {
        username,
        password,
        handleSubmit
      };
    }
  });
  </script>
  
  <style scoped>
  .login-container {
    max-width: 400px;
    margin: 100px auto;
    padding: 20px;
    box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
    border-radius: 8px;
    background-color: #fff;
  }
  
  h1 {
    text-align: center;
    margin-bottom: 20px;
    color: #333;
  }
  
  .form-group {
    margin-bottom: 15px;
  }
  
  label {
    display: block;
    margin-bottom: 5px;
    font-weight: bold;
  }
  
  input[type="text"],
  input[type="password"] {
    width: 100%;
    padding: 10px;
    border: 1px solid #ccc;
    border-radius: 5px;
    box-sizing: border-box;
    font-size: 16px;
  }
  
  input[type="text"]:focus,
  input[type="password"]:focus {
    border-color: #4a90e2;
    outline: none;
  }
  
  .login-button {
    width: 100%;
    padding: 10px;
    background-color: #4a90e2;
    color: white;
    font-size: 18px;
    border: none;
    border-radius: 5px;
    cursor: pointer;
    transition: background-color 0.3s ease;
  }
  
  .login-button:hover {
    background-color: #357abd;
  }
  
  .login-button:focus {
    outline: none;
  }
  </style>