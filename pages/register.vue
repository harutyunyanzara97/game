<template>
  <div class="min-h-screen flex items-center justify-center bg-gray-100">
    <form @submit.prevent="register" class="w-full max-w-sm p-8 bg-white rounded-lg shadow-md">
      <h2 class="text-2xl font-semibold text-center text-blue-600 mb-6">Register</h2>

      <!-- Phone Input -->
      <div class="mb-4">
        <label for="phone" class="block text-sm font-medium text-gray-700">Phone Number</label>
        <input v-model="phone" id="phone" type="text" placeholder="255XXXXXXXXX"
               class="mt-2 w-full p-3 border border-gray-300 rounded-md focus:outline-none focus:ring-2 focus:ring-blue-500">
      </div>

      <!-- Password Input -->
      <div class="mb-4">
        <label for="password" class="block text-sm font-medium text-gray-700">Password</label>
        <input v-model="password" id="password" type="password" placeholder="******"
               class="mt-2 w-full p-3 border border-gray-300 rounded-md focus:outline-none focus:ring-2 focus:ring-blue-500">
      </div>

      <!-- Confirm Password Input -->
      <div class="mb-6">
        <label for="confirmPassword" class="block text-sm font-medium text-gray-700">Confirm Password</label>
        <input v-model="confirmPassword" id="confirmPassword" type="password" placeholder="******"
               class="mt-2 w-full p-3 border border-gray-300 rounded-md focus:outline-none focus:ring-2 focus:ring-blue-500">
      </div>

      <!-- Register Button -->
      <button type="submit" class="w-full py-3 bg-blue-600 text-white rounded-md hover:bg-blue-700 focus:outline-none focus:ring-2 focus:ring-blue-500">Register</button>

      <!-- Link to Login -->
      <p class="mt-4 text-center text-sm text-gray-600">
        Already have an account? <nuxt-link to="/login" class="text-blue-600 hover:text-blue-700">Login here</nuxt-link>
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
const confirmPassword = ref('')

const register = async () => {
  try {
    const response = await axios.post('https://stagingnew.slotpesa.co.tz/api/v1/auth/Register', {
      phone: phone.value,
      password: password.value,
      confirmPassword: confirmPassword.value
    })
    const { token, code } = response.data
    // Call the verify registration API
    const verifyResponse = await axios.post('https://stagingnew.slotpesa.co.tz/api/v1/Auth/VerifyRegistration', {
      token,
      code
    })
    const { accessToken } = verifyResponse.data
    document.cookie = `accessToken=${accessToken}; path=/`
    await router.push('/game')
  } catch (error) {
    console.error('Registration failed:', error.response.data)
  }
}
</script>