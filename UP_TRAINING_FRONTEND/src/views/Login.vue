<script setup>
import { ref } from "vue";
import api from "../api";
import { useRouter } from "vue-router";
const router = useRouter();

const email = ref("");
const password = ref("");
const error = ref("");

async function login() {
    error.value = "";

    try {
        const res = await api.post("/login", {
            email: email.value,
            password: password.value,
        });

        localStorage.setItem("token", res.data.token);

        router.push("/profile"); // redirect
    } catch (err) {
        error.value = "Invalid email or password";
    }
}
</script>

<template>
    <div>
        <h1>Login</h1>

        <input v-model="email" type="email" placeholder="Email" />
        <input v-model="password" type="password" placeholder="Password" />

        <button @click="login">Login</button>

        <p v-if="error">{{ error }}</p>
    </div>
</template>
