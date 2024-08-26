<template>
    <div class="modal-overlay" v-show="open">
        <div class="modal-content">
            <button class="btn-close" @click="$emit('close')">X</button>
            <h1>{{ texto }}</h1>
            <h1>{{ id_burguer }}</h1>
            <div class="buttons">
                <button class="btn-1" @click="$emit('close')">{{ opcaofail }}</button>
                <router-link :to="rota">
                    <button class="btn-2" @click="onClick()">{{ opcaosuccess }}</button>
                </router-link>
            </div>
        </div>
    </div>
</template>

<script>
export default {
    name: 'Modal',
    props: {
        open: Boolean,
        texto: String,
        opcaofail: String,
        opcaosuccess: String,
        id_burguer: String,
        rota: String
    },
    methods: {
        onClick() {
            this.$emit('executarFuncao');

        }
    },
    setup(props, emit) {
        const closeModal = () => {
            emit('close', false)
        }
        return {
            closeModal
        }
    }
}
</script>

<style scoped>
h1 {
    font-size: 25px;
}

.modal-overlay {
    background-color: rgba(0, 0, 0, 0.5);
    z-index: 9998;
    @apply fixed top-0 left-0 w-[100%] h-[100%] flex justify-center items-center
}

.modal-content {
    box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
    @apply relative bg-[whitesmoke] p-3 rounded-lg flex flex-col justify-center items-center min-w-[40%] min-h-[40%]
}

.btn-close {
    transition: 0.5s;
    @apply bg-[#838383] text-white w-10 border-none py-[6px] px-[2px] rounded-lg cursor-pointer fixed top-[32%] left-[65%]
}

.btn-close:hover {
    background-color: #6d6d6d;
}

.buttons {
    @apply flex justify-center items-center w-[100%] text-center
}

.btn-1 {
    transition: 0.5s;
    @apply bg-[red] text-white w-10 border-none p-2 rounded-lg cursor-pointer m-2
}

.btn-1:hover {
    @apply bg-red-100 text-[red]
}

.btn-2 {
    transition: 0.5s;
    @apply bg-[green] text-white w-10 border-none p-2 rounded-lg cursor-pointer
}

.btn-2:hover {
    @apply bg-lime-100 text-[green]
}
</style>