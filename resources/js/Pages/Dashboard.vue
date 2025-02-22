<template>
    <div>
      <h1>Dashboard</h1>
      <p>Welcome, {{ user?.name }}</p>
      
      <button @click="logout">Logout</button>
    </div>
  </template>
  
  <script>
  import axios from 'axios';
  import { useRouter } from 'vue-router';
  
  export default {
    data() {
      return {
        user: null,
      };
    },
    setup() {
      const router = useRouter();
  
      const logout = async () => {
        try {
          await axios.post('/api/logout');
          localStorage.removeItem('token');
          axios.defaults.headers.common['Authorization'] = null;
          router.push('/login');
        } catch (error) {
          console.error('Logout failed:', error);
        }
      };
  
      return { logout };
    },
    async created() {
      try {
        const response = await axios.get('/api/user');
        this.user = response.data;
      } catch (error) {
        console.error('User fetch failed:', error);
      }
    }
  };
  </script>
  