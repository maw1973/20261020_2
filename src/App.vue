<script setup>
import { ref, computed } from 'vue'

const count = ref(0)
const name = ref('')
const nameLength = computed(() => Array.from(name.value).length)
const price = ref('150')
const quantity = ref('3')
const priceValid = computed(() => price.value.trim() !== '' && Number.isFinite(Number(price.value)) && Number(price.value) >= 0)
const quantityValid = computed(() => quantity.value.trim() !== '' && /^\d+$/.test(quantity.value) && Number(quantity.value) > 0 && Number.isSafeInteger(Number(quantity.value)))
const total = computed(() => priceValid.value && quantityValid.value ? Number(price.value) * Number(quantity.value) : null)
const submitted = ref(false)
function submitOrder() { submitted.value = true }
</script>

<template>
  <main class="page">
    <header><span class="eyebrow">Vue 3 · Vite · 課堂練習</span><h1>p.14 課堂練習：具體驗收</h1><p>計數器、姓名即時輸入與訂單小計</p></header>
    <section class="card"><h2>01｜計數器</h2><p>目前數值</p><div class="number">{{ count }}</div><div class="actions"><button @click="count++">+1</button><button @click="count += 2">+2</button><button class="secondary" @click="count = 0">歸零</button></div></section>
    <section class="card"><h2>02｜姓名輸入</h2><label for="name">姓名</label><input id="name" v-model="name" type="text" placeholder="例如：王小明"/><p>即時顯示：<strong>{{ name || '尚未輸入' }}</strong></p><p>目前字數：<strong>{{ nameLength }}</strong> 字</p><button class="secondary" @click="name = ''">清空姓名</button></section>
    <section class="card"><h2>03｜小計與驗證</h2><div class="fields"><div><label for="price">單價（元）</label><input id="price" v-model="price" type="text" inputmode="decimal" placeholder="請輸入單價"/><small v-if="!priceValid" class="error">請輸入有效的非負單價</small></div><div><label for="quantity">數量（正整數）</label><input id="quantity" v-model="quantity" type="text" inputmode="numeric" placeholder="請輸入數量"/><small v-if="!quantityValid" class="error">請輸入大於 0 的整數數量（不可輸入 2.5）</small></div></div><p class="subtotal">小計：<strong>{{ total === null ? '請先填寫有效資料' : `NT$ ${total.toLocaleString('zh-TW')}` }}</strong></p><button @click="submitOrder">確認送出</button><p v-if="submitted && total !== null" class="success">送出成功：小計 NT$ {{ total.toLocaleString('zh-TW') }}</p><p v-else-if="submitted" class="error">無法送出，請先修正輸入內容。</p></section>
    <footer>驗收操作：計數器 +1 兩次 → 2；+2 兩次 → 4；姓名「王小明」→ 3 字；單價 150、數量 3 → 450。</footer>
  </main>
</template>

<style scoped>
*{box-sizing:border-box}.page{max-width:850px;margin:auto;padding:32px 20px 65px;font-family:Arial,'Microsoft JhengHei',sans-serif;color:#1d2850}header{text-align:center;margin-bottom:30px}.eyebrow{color:#5970ad;font-size:13px;letter-spacing:.1em;font-weight:700}h1{font-size:clamp(25px,4vw,37px);margin:12px 0}header p{color:#6b7894}.card{background:#fff;padding:25px 29px;margin:18px 0;border-radius:18px;box-shadow:0 5px 24px #23365c15;border:1px solid #e5eaf4}h2{font-size:21px;margin:0 0 18px}label{display:block;font-weight:650;margin:9px 0 8px}input{width:100%;padding:12px 14px;font-size:17px;border:1px solid #b8c6df;border-radius:9px;outline-color:#3d72d9}button{background:#295bb9;border:0;border-radius:9px;color:white;padding:11px 20px;font-size:16px;cursor:pointer;margin:6px 8px 6px 0}button.secondary{background:#e7edf9;color:#1d407a}.number{font-size:64px;font-weight:800;color:#295bb9;margin:4px 0 12px}.fields{display:grid;grid-template-columns:1fr 1fr;gap:18px}.subtotal{font-size:20px;background:#eff5ff;padding:16px;border-radius:9px}.error{display:block;color:#b42332;margin-top:7px}.success{color:#16804a;font-weight:700}footer{color:#69768f;text-align:center;font-size:13px;line-height:1.6} @media(max-width:570px){.fields{grid-template-columns:1fr}.card{padding:20px}}
</style>
