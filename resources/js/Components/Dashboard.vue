<template>
    <div>
      <h1>Dashboard</h1>
      <p v-if="user">Welcome, {{ user.name }}</p>
      
      <button @click="logout">Logout</button>
      
      <router-link to="/messages">Go to Messages</router-link>
    </div>
</template>
  
<script>
import axios from 'axios';

export default {
  data() {
    return {
      user: null,
    };
  },
  async created() {
    await this.fetchUser();
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
        await axios.post('/api/logout', {}, {
          headers: { Authorization: `Bearer ${localStorage.getItem('auth_token')}` }
        });
        localStorage.removeItem('auth_token'); // Remove token
        delete axios.defaults.headers.common['Authorization']; // Remove from axios
        this.$router.push('/login'); // Redirect to login
      } catch (error) {
        console.error('Logout failed:', error);
      }
    },
  },
};
</script>
