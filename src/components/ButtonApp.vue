<template>
  <button type="submit" class="btn loading" :disabled="isLoading">
    <div v-if="!isLoading">Оставить заявку</div>
    <img class="btn__loading" v-else :src="require('@/assets/images/loading.png')" />
  </button>
</template>

<script>
export default {
  props: {
    isLoading: {
      type: Boolean,
      default: false,
    },
  },
}
</script>
