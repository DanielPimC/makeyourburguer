<template>
    <div class="main-container">
        <h1 style="font-size: 24px; font-weight: bold;">Lista de pedidos:</h1>
        <Dashboard />
    </div>
</template>

<script>
import Dashboard from '../components/Dashboard.vue'
export default {
    name: 'Pedidos',
    components: { 
        Dashboard
    }
}
</script>

<style scoped>

</style>