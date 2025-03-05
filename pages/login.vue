<template>
  <div class="min-h-screen flex items-center justify-center bg-gray-100">
    <form @submit.prevent="login" class="w-full max-w-sm p-8 bg-white rounded-lg shadow-md">
      <h2 class="text-2xl font-semibold text-center text-blue-600 mb-6">Login</h2>

      <!-- Phone Input -->
      <div class="mb-4">
        <label for="phone" class="block text-sm font-medium text-gray-700">Phone Number</label>
        <input v-model="phone" id="phone" type="text" placeholder="255XXXXXXXXX"
               class="mt-2 w-full p-3 border border-gray-300 rounded-md focus:outline-none focus:ring-2 focus:ring-blue-500">
      </div>

      <!-- Password Input -->
      <div class="mb-6">
        <label for="password" class="block text-sm font-medium text-gray-700">Password</label>
        <input v-model="password" id="password" type="password" placeholder="******"
               class="mt-2 w-full p-3 border border-gray-300 rounded-md focus:outline-none focus:ring-2 focus:ring-blue-500">
      </div>

      <!-- Login Button -->
      <button type="submit" class="w-full py-3 bg-blue-600 text-white rounded-md hover:bg-blue-700 focus:outline-none focus:ring-2 focus:ring-blue-500">Login</button>

      <!-- Link to Register -->
      <p class="mt-4 text-center text-sm text-gray-600">
        Don't have an account? <nuxt-link to="/register" class="text-blue-600 hover:text-blue-700">Register here</nuxt-link>
      </p>
    </form>
  </div>
</template>

<script setup>
import { ref } from 'vue'
import { useRouter } from 'vue-router'
import axios from 'axios'

const router = useRouter()
const phone = ref('')
const password = ref('')

const login = async () => {
  try {
    const response = await axios.post('https://stagingnew.slotpesa.co.tz/api/v1/auth/login', {
      phone: phone.value,
      password: password.value
    })
    const { token } = response.data
    document.cookie = `accessToken=${token}; path=/`
    await router.push('/game')
  } catch (error) {
    console.error('Login failed:', error.response.data)
  }
}
</script>
