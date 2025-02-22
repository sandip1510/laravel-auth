<template>
    <div class="register-container">
      <h2>Register</h2>
      <form @submit.prevent="register">
        <input v-model="name" type="text" placeholder="Full Name" required />
        <input v-model="email" type="email" placeholder="Email" required />
        <input v-model="password" type="password" placeholder="Password" required />
        <button type="submit">Register</button>
      </form>
      <p>Already have an account? <router-link to="/login">Login</router-link></p>
    </div>
  </template>
  
  <script>
  import axios from 'axios';
  export default {
    data() {
      return {
        name: '',
        email: '',
        password: ''
      };
    },
    methods: {
      async register() {
        try {
          await axios.post('/api/register', {
            name: this.name,
            email: this.email,
            password: this.password
          });
          alert('Registration successful! You can now log in.');
          this.$router.push('/login');
        } catch (error) {
          alert('Registration failed!');
        }
      }
    }
  };
  </script>
  