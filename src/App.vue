<template>
  <div>
    <Navbar :logo="logo_src" />
    <div>
      <router-view></router-view>
    </div>
    <Footer />
  </div>
</template>

<script>
import Navbar from './components/Navbar.vue'
import Footer from './components/Footer.vue'
export default {
  name: 'App',
  data() {
    return {
      logo_src: '/img/logo.png'
    }
  },
  components: {
    Navbar, 
    Footer
  }
}
</script>

<style>
  * {
    font-family: Helvetica;
    @apply m-0 p-0 box-border
  }

  .main-container {
    @apply m-[50px]
  }

  h1 { 
    @apply text-center mb-[30px] text-[#222]
  }
</style>