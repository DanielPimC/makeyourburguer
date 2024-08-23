<template>
    <div class="nav">
      <router-link to="/" class="logo">
          <img :src="logo" alt="Logo" id="logo" class="logo">
      </router-link>
      <router-link to="/">Home</router-link>
      <router-link to="/pedidos">Pedidos</router-link>
    </div>
  </template>

<script>
export default {
    name: 'Navbar', 
    props: ["logo"]
}
</script>

<style scoped>
  .nav{
    @apply bg-[#222] border-b-[4px] border-[#111] p-[15px_50px] flex justify-end items-center
  }

  .nav .logo {
    @apply m-auto ml-0 w-[40px] h-[40px]
  }

  .nav a {
    transition: 0.5s;
    @apply text-[#FCBA03] no-underline m-[12px]
  }

  .nav a:hover {
    @apply text-[#FFF]
  }
</style>