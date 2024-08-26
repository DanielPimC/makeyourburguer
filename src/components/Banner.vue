<template>
  <div class="main-banner">
    <h1>Make Your Burguer</h1>
  </div>

</template>

<script>
export default {
  name: 'Banner',
  data() {
    return {
      caminho: '/img/burger.jpg',
      alt: 'Imagem de Hamburguer'
    }
  }
}
</script>

<style scoped>
.main-banner {
  background-image: url('/img/burger.jpg');
  background-position: 0 -250px;
  background-size: cover;
  @apply h-[400px] flex items-center text-start
}

.main-banner h1 {
  @apply text-[#FFF] text-center bg-[#222] py-[20px] px-[40px] text-[42px]
}
</style>