<template>
    <div class="background" v-if="open">
        <h1>Hello World</h1>
        <button class="btn-close" @click="$emit('close')">Fechar Modal</button>
    </div>
</template>

<script>
export default {
    name: 'Modal',
    props: {
        open: Boolean
    },
    setup(props, emit) {
        const close = () => {
            emit('close', false)
        }
        return {
            closeModal
        }
    }
}
</script>

<style scoped>

</style>