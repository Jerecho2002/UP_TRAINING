<script setup>
import { ref, onMounted } from "vue";
import api from "../api";

const user = ref(null);
const error = ref("");

async function fetchProfile() {
    try {
        const res = await api.get("/profile");
        user.value = res.data;
    } catch (err) {
        error.value = "Failed to fetch profile";
    }
}

onMounted(() => {
    fetchProfile();
});

async function logout() {
    try {
        await api.post("/logout"); // call backend to delete token
    } catch (err) {
        console.error("Failed to logout from backend", err);
    }
    localStorage.removeItem("token"); // clear token locally
    window.location.href = "/login";  // redirect
}

</script>

<template>
    <div>
        <h1>Profile</h1>

        <p v-if="error">{{ error }}</p>

        <div v-if="user">
            <p><strong>Name:</strong> {{ user.name }}</p>
            <p><strong>Email:</strong> {{ user.email }}</p>
        </div>

        <button @click="logout">Logout</button>
    </div>
</template>
