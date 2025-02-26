<template></template>
<script setup>
import { onMounted } from 'vue'
import { useAuthStore} from '../store/auth'
import { useRouter } from 'vue-router

const authStore = useAuthStore()
const router = useRouter()

async function logout() {
    try {
        await authStore.logout()
    } catch(error) {
        console.error(error)
    }
}

async onMounted(() => {
    await authStore.fetchUser()
})
</script>
