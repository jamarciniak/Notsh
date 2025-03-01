<template>
  <div>
    <h2>Register</h2>
    <form @submit.prevent="register">
      <div>
        <label for="email">Email:</label>
        <input type="email" id="email" v-model="email" required />
      </div>
      <div>
        <label for="password">Password:</label>
        <input type="password" id="password" v-model="password" required />
      </div>
      <button type="submit">Register</button>
    </form>
    <p v-if="error">{{ error }}</p>
    <p v-if="success">{{ success }}</p>
  </div>
</template>
<script>
import { getCSRFToken } from "../store/auth";

export default {
  data() {
    return {
      email: "",
      password: "",
      error: "",
      success: "",
    };
  },
  methods: {
    async register() {
      try {
        const response = await fetch("http://localhost:8000/api/register", {
          method: "POST",
          headers: {
            "Content-Type": "application/json",
            "X-CSRFToken": getCSRFToken(),
          },
          body: JSON.stringify({
            email: this.email,
            password: this.password,
          }),
          credentials: "include",
        });
        const data = await response.json();

        if (response.ok) {
          this.success = "Registration successful! Please log in.";
          setTimeout(() => {
            this.$router.push("/login");
          }, 1000);
        } else {
          this.error = data.error || "Registration failed";
        }
      } catch (error) {
        this.error = "An error occurred during registration: " + error;
      }
    },
  },
};
</script>
