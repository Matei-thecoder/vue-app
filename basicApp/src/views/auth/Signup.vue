<template>
    <div class="signup-container">
      <h1>Signup</h1>
      <form @submit.prevent="handleSignup">
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
          <label for="email">Email:</label>
          <input
            type="email"
            id="email"
            v-model="email"
            placeholder="Enter your email"
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
  
        <button type="submit" class="signup-button">Signup</button>
        <p>If you already have an account, then <a href="/login">login</a>.</p>
      </form>
    </div>
  </template>
  
  <script lang="ts">
  import { defineComponent, ref } from 'vue';
  
  export default defineComponent({
    name: 'Signup',
    setup() {
      // Define reactive references for form fields
      const username = ref<string>('');
      const email = ref<string>('');
      const password = ref<string>('');
  
      // Handle form submission
      const handleSignup = () => {
        // Basic validation
        if (
          username.value.trim() === '' ||
          email.value.trim() === '' ||
          password.value.trim() === ''
        ) {
          alert('Please fill in all fields.');
          return;
        }
  
        if (!validateEmail(email.value)) {
          alert('Please enter a valid email address.');
          return;
        }
  
        // Simulate a signup process
        alert(`Signing up with username: ${username.value}, email: ${email.value}, and password: ${password.value}`);
        
        // Reset the form
        username.value = '';
        email.value = '';
        password.value = '';
      };
  
      // Simple email validation function
      const validateEmail = (email: string): boolean => {
        const re = /^[^\s@]+@[^\s@]+\.[^\s@]+$/;
        return re.test(email);
      };
  
      return {
        username,
        email,
        password,
        handleSignup
      };
    }
  });
  </script>
  
  <style scoped>
  .signup-container {
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
  input[type="email"],
  input[type="password"] {
    width: 100%;
    padding: 10px;
    border: 1px solid #ccc;
    border-radius: 5px;
    box-sizing: border-box;
    font-size: 16px;
  }
  
  input[type="text"]:focus,
  input[type="email"]:focus,
  input[type="password"]:focus {
    border-color: #4a90e2;
    outline: none;
  }
  
  .signup-button {
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
  
  .signup-button:hover {
    background-color: #357abd;
  }
  
  .signup-button:focus {
    outline: none;
  }
  </style>