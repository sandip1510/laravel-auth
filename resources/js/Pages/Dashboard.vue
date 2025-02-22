<template>
    <div>
      <h1>Dashboard</h1>
      <p>Welcome, {{ user?.name }}</p>
  
      <button @click="logout">Logout</button>
  
      <hr />
  
      <!-- Chat Section -->
      <div class="chat-container">
        <h2>Chatbox</h2>
  
        <!-- User List -->
        <div class="user-list">
          <h3>Users</h3>
          <ul>
            <li
              v-for="u in users"
              :key="u.id"
              @click="selectUser(u)"
              :class="{ active: selectedUser?.id === u.id }"
            >
              {{ u.name }}
            </li>
          </ul>
        </div>
  
        <!-- Messages -->
        <div class="messages" v-if="selectedUser">
          <h3>Chat with {{ selectedUser.name }}</h3>
          <div v-for="msg in messages" :key="msg.id" class="message">
            <strong>{{ msg.sender.name }}:</strong> {{ msg.message }}
          </div>
          <input v-model="newMessage" placeholder="Type a message..." @keyup.enter="sendMessage" />
          <button @click="sendMessage">Send</button>
        </div>
      </div>
    </div>
  </template>
  
  <script>
  import axios from "axios";
  import { useRouter } from "vue-router";
  
  export default {
    data() {
      return {
        user: null,
        users: [], // User list
        selectedUser: null, // Selected chat user
        messages: [], // Messages
        newMessage: "", // New message input
      };
    },
    setup() {
      const router = useRouter();
  
      const logout = async () => {
        try {
          await axios.post("/api/logout");
          localStorage.removeItem("token");
          axios.defaults.headers.common["Authorization"] = null;
          router.push("/login");
        } catch (error) {
          console.error("Logout failed:", error);
        }
      };
  
      return { logout };
    },
    async created() {
      try {
        const response = await axios.get("/api/user");
        this.user = response.data;
  
        // Fetch all users
        const usersResponse = await axios.get("/api/users");
        this.users = usersResponse.data;
      } catch (error) {
        console.error("Data fetch failed:", error);
      }
    },
    methods: {
      async selectUser(user) {
        this.selectedUser = user;
        await this.fetchMessages(user.id);
      },
      async fetchMessages(receiverId) {
        try {
          const response = await axios.get(`/api/messages/${receiverId}`);
          this.messages = response.data;
        } catch (error) {
          console.error("Failed to fetch messages:", error);
        }
      },
      async sendMessage() {
        if (!this.newMessage.trim()) return;
  
        try {
            console.log("*****Send messgae****");
            console.log(this.selectedUser.id);
            
            
          await axios.post("/api/messages", {
            receiver_id: this.selectedUser.id,
            message: this.newMessage,
          });
  
          this.newMessage = "";
          this.fetchMessages(this.selectedUser.id);
        } catch (error) {
          console.error("Message sending failed:", error);
        }
      },
    },
  };
  </script>
  
  <style scoped>
  .chat-container {
    display: flex;
    gap: 20px;
    margin-top: 20px;
  }
  .user-list {
    width: 30%;
    border-right: 1px solid #ccc;
    padding-right: 10px;
  }
  .user-list ul {
    list-style: none;
    padding: 0;
  }
  .user-list li {
    cursor: pointer;
    padding: 5px;
  }
  .user-list li.active {
    font-weight: bold;
    background: #ddd;
  }
  .messages {
    width: 70%;
  }
  .message {
    padding: 5px 0;
  }
  input {
    width: 100%;
    padding: 5px;
    margin-top: 10px;
  }
  </style>
  