<template>
    <div class="col-md-8">
        <h2 class="fs-4 fw-bold mb-3">商品列表</h2>
        <div class="row">
          <div v-for="item in props.productList" :key="item.id" class="col-md-4 mb-4">
            <div class="card h-100">
              <img :src="item.img" class="card-img-top">
              <div class="card-body">
                <h5 class="card-title fs-5 fw-bold">{{ item.name }}</h5>
                <p class="card-text">{{ item.describe }}</p>
                <p class="fw-bold text-primary my-2">$ {{ item.price }}</p>
                <button class="btn btn-success w-100" @click="addCartItem(item.id)"> 加入購物車 </button>
              </div>
            </div>
          </div>
        </div>
    </div>    
</template>

<script setup>
const props = defineProps({
  productList: { type: Array, required: true }
})

const emit = defineEmits(['emit-product'])

const addCartItem = (productId) => {
  emit('emit-product', productId)
}
</script>