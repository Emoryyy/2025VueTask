<template>
    <div class="col-md-4">
      <h2 class="fs-4 fw-bold mb-3">購物車</h2>
      <ul class="list-group mb-3">
        <li v-if="props.cartList && props.cartList.length > 0" v-for="(item, index) in props.cartList" :key="item.id" class="list-group-item d-flex justify-content-between align-items-center">
          <div>
            <h6 class="my-1">{{ item.name }}</h6>
            <small class="text-muted">數量：{{ item.num }}</small>
          </div>
          <div>
            <span class="text-muted">${{ item.price }}</span>
            <button class="btn btn-sm btn-outline-danger ms-2" @click="removeCartItem(item.id)"> 移除 </button>
          </div>
        </li>
        <li v-else class="list-group-item d-flex justify-content-between align-items-center">購物車內無商品</li>
      </ul>
      <div class="text-end">總金額：{{ props.priceTotal }}</div>
    </div>
</template>

<script setup>
const props = defineProps({
  cartList: { type: Array, required: true },
  priceTotal: {type: Number, required: true },
})

const emit = defineEmits(['emit-product'])

const removeCartItem = (productId) => {
  emit('emit-product', productId)
}
</script>