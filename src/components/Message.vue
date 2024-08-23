<template>
    <div class="message-container">
        <p>{{ msg }}</p>
    </div>
</template>

<script>
export default {
    name: 'Message',
    props: {
        msg: String
    }
}
</script>

<style scoped>
    .message-container {
        border: 2px solid #B8DAFF;
        margin: 30px auto;
        text-align: center;
        @apply text-[#004085] bg-[#CCE5FF] rounded-md p-[10px] w-[400px]
    }
</style>