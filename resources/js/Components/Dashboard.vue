<template>
    <div>
      <h1>Dashboard</h1>
      <p>Welcome, {{ user.name }}</p>
      
      <button @click="logout">Logout</button>
      
      <router-link to="/messages">Go to Messages</router-link>
    </div>
  </template>
  
  <script>
  import axios from 'axios';
  
  export default {
    data() {
      return {
        user: {},
      };
    },
    created() {
      this.fetchUser();
    },
    methods: {
      async fetchUser() {
        try {
          const response = await axios.get('/api/user');
          this.user = response.data;
        } catch (error) {
          console.error('Error fetching user:', error);
        }
      },
      async logout() {
        try {
          await axios.post('/api/logout');
          localStorage.removeItem('auth_token'); // Clear token
          this.$router.push('/login'); // Redirect to login
        } catch (error) {
          console.error('Logout failed:', error);
        }
      },
    },
  };
  </script>
  