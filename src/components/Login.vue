<template>
  <h1>登入頁面</h1>
  <form class="login-form">
    <div class="input-section">
      <label class="input-label">帳號:</label>
      <input type="text" class="login-input" v-model="username">
    </div>
    <div class="input-section">
      <label class="input-label">密碼:</label>
      <input type="password" class="login-input" v-model="password">
    </div>
    <button class="submit-btn" @click.prevent="login">登入</button>
  </form>
</template>

<script setup>
import { ref, onMounted } from 'vue'
import { useRouter } from "vue-router";

const router = useRouter()

const username = ref("")
const password = ref("")
const storageAccess = sessionStorage.getItem("access_token")
const storageId = sessionStorage.getItem("id_token")
const access_token = "eyJraWQiOiJHcGlcL0ZVaTZpXC9iOUw4SzZOb3FKSUo5bWlYdTZOY3FybXNublNhQ0srOUE9IiwiYWxnIjoiUlMyNTYifQ.eyJzdWIiOiIwZmY3NjAyZS1jZWFlLTQ2Y2QtYjI3MC0zZjBkZGM4NWI1NTAiLCJjb2duaXRvOmdyb3VwcyI6WyJ0ZXN0Il0sImlzcyI6Imh0dHBzOlwvXC9jb2duaXRvLWlkcC5hcC1ub3J0aGVhc3QtMS5hbWF6b25hd3MuY29tXC9hcC1ub3J0aGVhc3QtMV9hSnRwcUpzUFEiLCJjbGllbnRfaWQiOiI2djNncnRwY3M1OXEwaTV2bXF2dXVhZGFociIsIm9yaWdpbl9qdGkiOiI4N2YxNDI5Ni0yMjUyLTQ1NWQtODBiZC1kZTk4MDA1YWYxMzYiLCJldmVudF9pZCI6IjM3NDAzYWYwLWY4M2UtNGU4MC1hMDE5LWFmNTA5MjA4ZDI5MSIsInRva2VuX3VzZSI6ImFjY2VzcyIsInNjb3BlIjoiYXdzLmNvZ25pdG8uc2lnbmluLnVzZXIuYWRtaW4iLCJhdXRoX3RpbWUiOjE2ODIzMjk5NzQsImV4cCI6MTY4MjQxNjM3NCwiaWF0IjoxNjgyMzI5OTc0LCJqdGkiOiJlNDU2MWExNi04OWRjLTQ3ZDQtYmM1Ni1hOTc2ZmQ0ZWY2ODUiLCJ1c2VybmFtZSI6ImZyYW5rdGVzdCJ9.SHis6aoYhKSTb1VdeOAf2ssaEXxr-rmNUkJ3-gFdBtUFsI_84FBM84Z007crjkAQ0QfRoAivwxmJQeSiby06_jWRA2H9GxzSIZKSfjen-Tr7t8Lyhr-4-Adb-othgZiCPMiCtS0OB7P8yDVZl6De0YTP8AZjkJk2LnwjZ8k_4u8nXQo4BOEUNzD6R6VWMELoRH06OkjgYAKgPCzp9WSp7xzImjZ6sYPavRbN15H-wZpWtwmYYOHjx5pciLkmVeOd6d-2p7jsI-Esp1NwAFq8oUu_AEsd7tOyrQ-_9ZeGRzn-Lxudw1jU6yGAl42onBavAPpuRCQdjXd83kXJcG87Iw"
const id_token = "eyJraWQiOiI1dEF3clQ3XC91cDg1XC9jdVFoSm13WGJtVmwrMFYrZUhOXC9tY3JkMHliVnlBPSIsImFsZyI6IlJTMjU2In0.eyJzdWIiOiIwZmY3NjAyZS1jZWFlLTQ2Y2QtYjI3MC0zZjBkZGM4NWI1NTAiLCJjb2duaXRvOmdyb3VwcyI6WyJ0ZXN0Il0sImVtYWlsX3ZlcmlmaWVkIjp0cnVlLCJpc3MiOiJodHRwczpcL1wvY29nbml0by1pZHAuYXAtbm9ydGhlYXN0LTEuYW1hem9uYXdzLmNvbVwvYXAtbm9ydGhlYXN0LTFfYUp0cHFKc1BRIiwiY29nbml0bzp1c2VybmFtZSI6ImZyYW5rdGVzdCIsIm9yaWdpbl9qdGkiOiI4N2YxNDI5Ni0yMjUyLTQ1NWQtODBiZC1kZTk4MDA1YWYxMzYiLCJhdWQiOiI2djNncnRwY3M1OXEwaTV2bXF2dXVhZGFociIsImV2ZW50X2lkIjoiMzc0MDNhZjAtZjgzZS00ZTgwLWEwMTktYWY1MDkyMDhkMjkxIiwidG9rZW5fdXNlIjoiaWQiLCJjdXN0b206Y2RiX2lkIjoiZmlyY2RwLWRldiIsImF1dGhfdGltZSI6MTY4MjMyOTk3NCwibmFtZSI6ImZyYW5rIiwiY3VzdG9tOmF1dGhvcml6YXRpb24iOiJwaGFzZTQiLCJleHAiOjE2ODI0MTYzNzQsImlhdCI6MTY4MjMyOTk3NCwianRpIjoiNjU2M2YyYmUtM2RlZS00MzNiLWFiYzQtYzRhMTBkZmRlZTM2IiwiZW1haWwiOiJuZWxzb25AcmV0YWlsaW5nZGF0YS5jb20udHcifQ.CKQAcBpL4FkX3v9NKwwH5Eg_JVMQ6Htpssm3D5VGO7fmX9MFD0R84mxQaJBG0juSqhOgmpK__QUsAHiDpJjsLKEo9tyJbetCWfxdYau9TCCMFQ48IZk7Bnse74OszQgKrl3sw3OgX8lYSoCFMYrzhY7NtfaneQfBVN3Ave7Jl6tlKSyFjzN4f2XI-ZRa6zG3o9V5XFx6teViC6h6ndTI9LoWXpVncSG8rEboW4cjF8AVJCp-rJYK4FNZc4PbWfWirMti4zITuB20zTmoEdVwce1ShzR6eOCaRfgzfH6KtS_lV48r6Bp044tnspIgplk5SuoLx3hLg4VPd8AxjU7BjA"

function login() {
  if (username.value === "franktest" && password.value === "Bb123456789") {
    sessionStorage.setItem("access_token", access_token)
    sessionStorage.setItem("id_token", id_token)
    router.push('./Home')
  }
}

onMounted(() => {
  if (storageAccess === access_token && storageId === id_token) {
    router.push('./Home')
  }
})
</script>

<style scoped>
.login-form {
  display: flex;
  flex-direction: column;
}

label {}

.login-input {}

.input-section {
  display: flex;
  flex-direction: column;
  font-size: 28px;
  margin-bottom: 24px;
}

.input-label {
  text-align: left;
}

input {
  font-size: 28px;
}

.submit-btn {
  width: 100%;
}
</style>
