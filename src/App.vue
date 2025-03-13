<script setup>
import { shallowRef, ref, onMounted } from 'vue';
import Login from './components/Login.vue';
import Signup from './components/Signup.vue';
import Notes from './components/Notes.vue';

const status = shallowRef(null); // Store the current status
const isLoggedIn = ref(false);

// Check if the user is logged in
function checkLoginStatus() {
  const accessToken = localStorage.getItem("access_token");
  isLoggedIn.value = !!accessToken; // Set to true if accessToken exists
}

// Show Signup component
function showSignup() {
  status.value = Signup;
}

// Show Login component
function showLogin() {
  status.value = Login;
}

// Logout the user
function logout() {
  localStorage.removeItem("access_token");
  localStorage.removeItem("refresh_token");
  isLoggedIn.value = false; // Update login status
  status.value = ""; // Reset the displayed component
}

// Check login status when the component mounts
onMounted(checkLoginStatus);
</script>

<template>
  <!-- Show Signup and Login buttons only if the user is not logged in -->
  <div v-if="!isLoggedIn">
    <button @click="showSignup">Signup</button>
    <button @click="showLogin">Login</button>
  </div>

  <!-- Show Logout button only if the user is logged in -->
  <div v-if="isLoggedIn">
    <button @click="logout">Logout</button>
  </div>

  <main>
    <!-- Show Signup or Login component based on status -->
    <component :is="status" v-if="!isLoggedIn" />

    <!-- Show Notes component if the user is logged in -->
    <Notes v-if="isLoggedIn" />
  </main>
</template>

<style scoped>
button {
  margin: 5px;
  padding: 10px 15px;
  background: #007bff;
  color: white;
  border: none;
  border-radius: 4px;
  cursor: pointer;
  font-size: 14px;
}

button:hover {
  background: #0056b3;
}

main {
  margin-top: 20px;
}
</style>