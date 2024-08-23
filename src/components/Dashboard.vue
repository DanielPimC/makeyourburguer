<template>
    <div class="burguer-table">
        <Message :msg="msg" v-show="msg" />
        <div>
            <div class="burguer-table-heading">
                <div class="order-id">#:</div>
                <div>Cliente:</div>
                <div>Pão:</div>
                <div>Carne:</div>
                <div>Opcionais:</div>
                <div>Ações: </div>
            </div>
        </div>
        <div class="burguer-table-rows">
            <div class="burguer-table-row" v-for="burguer in burguers" :key="burguer.id">
                <div class="order-number">{{ burguer.id }}</div>
                <div>{{ burguer.nome }}</div>
                <div>{{ burguer.pao }}</div>
                <div>{{ burguer.carne }}</div>
                <div>
                    <ul>
                        <li v-for="(opcional, index) in burguer.opcionais" :key="index">
                            {{ opcional }}
                        </li>
                    </ul>
                </div>
                <div>
                    <select name="status" class="status" @change="updateBurguer($event, burguer.id)">
                        <option value="">Status do pedido</option>
                        <option v-for="stats in status" :key="stats.id" :selected="burguer.status == stats.tipo"
                            :value="stats.tipo">{{ stats.tipo }}</option>
                    </select>
                    <button class="delete-btn" @click="deletarPedido(burguer.id)">Cancelar pedido</button>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
import axios from 'axios'
import Message from './Message.vue'
import Modal from './Modal.vue'
import { ref } from 'vue'
export default {
    name: 'Dashboard',
    data() {
        return {
            burguers: null,
            burguer_id: null,
            status: [],
            msg: '',
            showModal: false
        }
    },
    components: {
        Message,
        Modal
    },
    methods: {
        async getPedidos() {
            const res = await axios.get('http://localhost:3000/burgers');
            console.log(res.data)
            this.burguers = res.data
            this.getStatus()
        },
        async getStatus() {
            const res = await axios.get('http://localhost:3000/status')
            console.log(res.data)
            this.status = res.data
        },
        async deletarPedido(id) {
            const res = await axios.delete(`http://localhost:3000/burgers/${id}`)
            console.log(res.data)
            this.getPedidos()
            this.msg = `Pedido N'º ${id} deletado com sucesso!`
            setTimeout(() => {
                 this.msg = ''
            }, 5000);
        },
        async updateBurguer(event, id) {
            const option = event.target.value;
            const dataJson = JSON.stringify({ status: option });
            const req = await axios.patch(`http://localhost:3000/burgers/${id}`, dataJson);
            const res = req.data
            console.log(res)
            this.msg = `Pedido N'º ${res.id} atualizado para ${res.status} com sucesso!`
            setTimeout(() => {
                 this.msg = ''
            }, 5000);
        }
    },
    mounted() {
        this.getPedidos()
    },
    setup() {
        const isOpen = ref(false)

        const openModal = () => {
            isOpen.value = true

            return isOpen.value
        }

        return {
            isOpen, openModal
        }
    }
}
</script>

<style scoped>
.burguer-table {
    margin: 0 auto;
    @apply max-w-[1200px]
}

.burguer-table-heading,
.burguer-table-rows,
.burguer-table-row {
    @apply flex flex-wrap
}

.burguer-table-heading {
    border-bottom: 3px solid #333;
    @apply font-bold p-[12px]
}

.burguer-table-heading div,
.burguer-table-row div {
    @apply w-[19%]
}

.burguer-table-row {
    border-bottom: 1px solid #CCC;
    @apply w-[100%] p-[12px]
}

.burguer-table-heading .order-id,
.burguer-table-row .order-number {
    @apply w-[5%]
}

select {
    @apply py-[12px] px-[6px] mr-12
}

.delete-btn {
    border: 2px solid #222;
    font-size: 16px;
    margin: 0 auto;
    transition: 0.5s;
    @apply bg-[#222] text-[#FCBA03] font-bold p-[10px] cursor-pointer rounded-md
}

.delete-btn:hover {
    @apply bg-transparent text-[#333]
}
</style>