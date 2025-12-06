<script setup>
import { ref } from "vue";
import api from "../api";

const name = ref("");
const email = ref("");
const password = ref("");
const error = ref("");

async function register() {
    error.value = "";

    try {
        // 1. Register the user
        await api.post("/register", {
            name: name.value,
            email: email.value,
            password: password.value
        });

        // 2. Automatically log in the user
        const loginRes = await api.post("/login", {
            email: email.value,
            password: password.value
        });

        // 3. Store the token in localStorage
        localStorage.setItem("token", loginRes.data.token);

        // 4. Redirect to profile
        window.location.href = "/profile";

    } catch (err) {
        if (err.response && err.response.data && err.response.data.errors) {
            // Validation errors from Laravel
            error.value = Object.values(err.response.data.errors).flat().join(", ");
        } else if (err.response && err.response.data && err.response.data.message) {
            error.value = err.response.data.message;
        } else {
            error.value = "Failed to register";
        }
    }
}
</script>

<template>
    <div>
        <h1>Register</h1>

        <input v-model="name" type="text" placeholder="Name" />
        <input v-model="email" type="email" placeholder="Email" />
        <input v-model="password" type="password" placeholder="Password" />

        <button @click="register">Register</button>

        <p v-if="error" style="color: red;">{{ error }}</p>
    </div>
</template>
