<template>
    <div class="flex min-h-screen items-center justify-center bg-gradient-to-r from-blue-500 to-purple-600 p-6">
      <div class="w-full max-w-md bg-white rounded-2xl shadow-2xl p-8 transform transition duration-500 hover:scale-105">
        <h2 class="text-3xl font-extrabold text-gray-800 text-center mb-6">Login</h2>
        <form @submit.prevent="login" class="space-y-5">
          <div>
            <label class="block text-gray-600 font-semibold mb-1">Email</label>
            <input v-model="email" type="email" placeholder="Enter your email" required
              class="w-full px-4 py-2 border rounded-lg focus:outline-none focus:ring-2 focus:ring-purple-500 shadow-sm" />
          </div>
          <div>
            <label class="block text-gray-600 font-semibold mb-1">Password</label>
            <input v-model="password" type="password" placeholder="Enter your password" required
              class="w-full px-4 py-2 border rounded-lg focus:outline-none focus:ring-2 focus:ring-purple-500 shadow-sm" />
          </div>
          <button type="submit"
            class="w-full bg-purple-600 hover:bg-purple-700 text-white font-semibold py-3 rounded-lg transition duration-300 shadow-md">
            Login
          </button>
        </form>
        <p class="text-gray-700 text-center mt-6">
          Don't have an account?
          <router-link to="/register" class="text-purple-600 hover:underline font-semibold">Register</router-link>
        </p>
      </div>
    </div>
  </template>
  
  <script>
  import axios from 'axios';
  export default {
    data() {
      return {
        email: '',
        password: ''
      };
    },
    methods: {
      async login() {
        try {
          const response = await axios.post('/api/login', {
            email: this.email,
            password: this.password
          });
          localStorage.setItem('token', response.data.access_token);
          axios.defaults.headers.common['Authorization'] = `Bearer ${response.data.access_token}`;
          this.$router.push('/dashboard');
        } catch (error) {
          alert('Login failed! Please check your credentials.');
        }
      }
    }
  };
  </script>