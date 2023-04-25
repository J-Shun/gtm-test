<template>
  <div class="info-section">
    <form class="login-form">
      <h2>註冊測試</h2>
      <div class="input-section">
        <label for="customerId" class="input-label">用戶 ID：</label>
        <input id="customerId" type="text" class="login-input" v-model="customerId">
      </div>
      <div class="input-section">
        <label for="customerName" class="input-label">用戶名稱：</label>
        <input id="customerName" type="text" class="login-input" v-model="customerName">
      </div>
      <div class="input-section">
        <label for="customerEmail" class="input-label">用戶 Email：</label>
        <input id="customerEmail" type="text" class="login-input" v-model="customerEmail">
      </div>
      <div class="input-section">
        <label for="customerMobile" class="input-label">用戶手機：</label>
        <input id="customerMobile" type="text" class="login-input" v-model="customerMobile">
      </div>
      <div class="btn-group">
        <button class="submit-btn" @click.prevent="register">註冊</button>
        <button class="submit-btn" @click.prevent="login">登入</button>
      </div>
    </form>

    <div class="login-info">
      <h2>登入資料</h2>
      <div>
        <p>用戶 ID： {{ loginId }}</p>
        <p>用戶名稱： {{ loginName }}</p>
        <p>用戶 Email： {{ loginEmail }}</p>
        <p>用戶手機： {{ loginMobile }}</p>
      </div>
    </div>
  </div>

  <hr>

  <h2>購物車、購買、轉換測試</h2>
  <select id="cart-selection" class="cart-selection" v-model="selectedProduct">
    <option value="" disabled selected hidden>選擇商品</option>
    <option value="阿里山蓮汁雪耳燉135ml">阿里山蓮汁雪耳燉135ml</option>
    <option value="桂冠凱薩醬100g">桂冠凱薩醬100g</option>
    <option value="冷凍訂單滿799元 送大福冰淇淋乙顆 (花生/芝麻口味隨機)">冷凍訂單滿799元 送大福冰淇淋乙顆 (花生/芝麻口味隨機)</option>
    <option value="奶黃包 70g25入">奶黃包 70g25入</option>
  </select>
  <p class="selected-product">選擇商品： {{ selectedProduct }}</p>
  <button class="add-cart" @click="addCart">加入購物車</button>

  <div class="product-section">
    <div class="product-box">
      <h2>購物車清單</h2>
      <ul>
        <li v-for="(item, index) in cartProducts" :key="index">
          {{ item }}
          <span>{{ (index + 1) * 10 }} 元</span>
        </li>
      </ul>
    </div>

    <div class="product-box">
      <h2>購買商品</h2>
      <ul>
        <li v-for="(item, index) in purchaseProducts" :key="index">
          {{ item }}
          <span>{{ (index + 1) * 10 }} 元</span>
        </li>
      </ul>
    </div>
  </div>

  <div class="btn-group">
    <button class="clear" @click="clear">清除</button>
    <button class="purchase" @click="purchase">下訂</button>
  </div>
</template>

<script setup>
import { ref } from 'vue'

const customerId = ref("")
const customerName = ref("")
const customerEmail = ref("")
const customerMobile = ref("")

const loginId = ref("")
const loginName = ref("")
const loginEmail = ref("")
const loginMobile = ref("")

function register() {
  loginId.value = customerId.value
  loginName.value = customerName.value
  loginEmail.value = customerEmail.value
  loginMobile.value = customerMobile.value
}

function login() {
  loginId.value = customerId.value
  loginName.value = customerName.value
  loginEmail.value = customerEmail.value
  loginMobile.value = customerMobile.value
}

const selectedProduct = ref("")
const cartProducts = ref([])
const purchaseProducts = ref([])

function addCart() {
  if (selectedProduct.value === "") return alert('無商品')
  cartProducts.value = [...cartProducts.value, selectedProduct.value]
  sessionStorage.setItem("customer_id", customerId.value)
}

function purchase() {
  if (cartProducts.value.length === 0) return alert('無商品')
  purchaseProducts.value = [...purchaseProducts.value, ...cartProducts.value]
  cartProducts.value = []
  sessionStorage.setItem("customer_id", customerId.value)
}

function clear() {
  purchaseProducts.value = []
  cartProducts.value = []
}
</script>

<style scoped>
.info-section {
  display: flex;
  gap: 20px;
}

hr {
  margin-bottom: 80px;
}

h2 {
  font-size: 20px;
}

.login-form {
  display: flex;
  flex-direction: column;
  width: 100%;
  margin-bottom: 80px;
}

.login-info {
  width: 100%;
}

.input-section {
  display: flex;
  flex-direction: column;
  font-size: 18px;
  margin-bottom: 24px;
}

.input-label {
  text-align: left;
}

input {
  font-size: 18px;
}

.submit-btn {
  width: 100%;
}

.default-btn {
  width: 100%;
}

.cart-selection {
  font-size: 16px;
  padding: 8px 4px;
  margin-bottom: 12px;
}

.selected-product {
  font-size: 18px;
}

.add-cart {
  margin-bottom: 48px;
}

.product-section {
  display: flex;
  gap: 32px;
  margin-bottom: 32px;
}

.product-box {
  width: 100%;
  height: 300px;
  max-height: 300px;
  border: 1px solid #000;
  border-radius: 10px;
  padding: 8px 16px;
  overflow-y: auto;
}

.product-box::-webkit-scrollbar {
  width: 7px;
}

.product-box::-webkit-scrollbar-button {
  background: transparent;
  border-radius: 4px;
}

.product-box::-webkit-scrollbar-track-piece {
  background: transparent;
}

.product-box::-webkit-scrollbar-thumb {
  border-radius: 4px;
  background-color: rgba(0, 0, 0, 0.4);
  border: 1px solid slategrey;
}

.product-box::-webkit-scrollbar-track {
  box-shadow: transparent;
}

ul {
  list-style: none;
  padding: 0;
}

li {
  margin-bottom: 4px;
}

.btn-group {
  display: flex;
  justify-content: center;
  align-items: center;
  gap: 8px;
}

.clear {
  width: 100%;
  border: 1px solid #ec5757;
  color: #ec5757;
}

.clear:active {
  border: 1px solid #ec5757;
}

.purchase {
  width: 100%;
  background: #18557e;
  color: #fff;
}
</style>