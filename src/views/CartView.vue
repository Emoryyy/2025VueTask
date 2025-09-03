<template>
  <div id="app" class="container py-4">
    <div class="row">
      <!-- 商品列表區 -->
      <ProductList :productList="pageProductList" @emit-product="addProduct" />

      <!-- 購物車區 -->
      <CartList :cartList="pageCartList" @emit-product="removeProduct" :priceTotal="cartPriceTotal" />
    </div>

    <!-- 通知元件 -->
    <NotifyMsg />
  </div>
</template>

<script setup>
import { computed, ref, provide } from 'vue';
import ProductList from '../components/ProductList.vue'
import CartList from '../components/CartList.vue'
import NotifyMsg from '../components/NotifyMsg.vue'

// 通知訊息
const notifyMsg = ref({
  message: '',
  isShow: false,
  method: 'add'
})
provide("notifyMsg", notifyMsg.value);

const showNotifyMsg = (msg, method) => {
  notifyMsg.value.message = msg
  notifyMsg.value.isShow = true
  notifyMsg.value.method = method
  setTimeout(() => {
    notifyMsg.value.isShow = false
  }, 1000)
}

// 加入商品
const addProduct = (productId) => {
  if(productId) {
    const producItem = pageProductList.value.find(item => item.id == productId)
    const cartIndex = pageCartList.value.findIndex(item => item.id == productId)
    if (cartIndex > -1) {
      pageCartList.value[cartIndex].num += 1
    }
    else {
      pageCartList.value.push({
        id: producItem.id,
        name: producItem.name,
        price: producItem.price,
        num : 1
      })
    }
    showNotifyMsg(producItem.name, 'add')
  }
}

// 移除商品
const removeProduct = (productId) => {
  if(productId) {
      const productIndex = pageCartList.value.findIndex(item => item.id == productId)
      if(productIndex > -1) {
        const productName = pageCartList.value[productIndex].name
        pageCartList.value.splice(productIndex, 1)
        showNotifyMsg(productName, 'remove')
      }
  }
}

// 計算總金額
const cartPriceTotal = computed(() => {
  const total = pageCartList.value.reduce((sum, item) => sum + item.price * item.num, 0)
  return total
})

const pageProductList = ref([
  {
    id: 1,
    img: 'https://images.unsplash.com/photo-1546435770-a3e426bf472b?q=80&amp;w=2065&amp;auto=format&amp;fit=crop&amp;ixlib=rb-4.1.0&amp;ixid=M3wxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8fA%3D%3D',
    name: '耳罩式藍牙耳機-藍色',
    describe: '舒適配戴，支援降噪技術',
    price: 2490
  },
  {
    id: 2,
    img: 'https://images.unsplash.com/photo-1546435770-a3e426bf472b?q=80&amp;w=2065&amp;auto=format&amp;fit=crop&amp;ixlib=rb-4.1.0&amp;ixid=M3wxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8fA%3D%3D',
    name: '耳罩式藍牙耳機-紅色',
    describe: '舒適配戴，支援降噪技術',
    price: 2480
  },
  {
    id: 3,
    img: 'https://images.unsplash.com/photo-1546435770-a3e426bf472b?q=80&amp;w=2065&amp;auto=format&amp;fit=crop&amp;ixlib=rb-4.1.0&amp;ixid=M3wxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8fA%3D%3D',
    name: '耳罩式藍牙耳機-黃色',
    describe: '舒適配戴，支援降噪技術',
    price: 2470
  },
])

const pageCartList = ref([])


</script>

<style scoped>
  body {
    background: #f2f2f2f2;
  }

  .card-img-top {
    height: 150px;
    object-fit: cover;
  }
</style>
