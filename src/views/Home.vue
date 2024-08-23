<template>
    <Banner />
    <div class="main-container"></div>
    <FormBurguer />
</template>

<script>
import Banner from '../components/Banner.vue'
import FormBurguer from '../components/FormBurguer.vue'
export default {
    name: 'Home',
    components: {
        Banner,
        FormBurguer
    }
}
</script>