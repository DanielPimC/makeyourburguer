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
                    <button class="delete-btn" @click="abrirModal(burguer.id)">Cancelar pedido</button>
                </div>
            </div>
            <Modal class="modal-container" :open="aberto" @close="aberto = !aberto" texto="Deseja deletar o pedido?"
                :id_burguer="burguer_id" @executarFuncao=deletarPedido(burguer_id) opcaofail="não" opcaosuccess="sim" />
        </div>
    </div>
</template>

<script>
import axios from 'axios'
import Message from './Message.vue'
import Modal from './Modal.vue'

export default {
    name: 'Dashboard',
    data() {
        return {
            burguers: null,
            burguer_id: null,
            status: [],
            msg: '',
            showModal: false,
            texto: '',
            aberto: false
        }
    },
    components: {
        Message,
        Modal
    },
    methods: {
        async getPedidos() {
            try {
                const res = await axios.get('http://localhost:3000/burgers');
                console.log(res.data)
                this.burguers = res.data
                this.getStatus()
            } catch (error) {
                console.error(error)
            }
        },
        async getStatus() {
            try {
                const res = await axios.get('http://localhost:3000/status')
                console.log(res.data)
                this.status = res.data
            } catch (error) {
                console.error(error)
            }
        },
        async deletarPedido(id) {
            try {
                const res = await axios.delete(`http://localhost:3000/burgers/${id}`)
                console.log(res.data)
                this.getPedidos()
                this.msg = `Pedido Nº ${id} deletado com sucesso!`
                setTimeout(() => {
                    this.msg = ''
                }, 2000);
                this.aberto = false
            } catch (error) {
                console.error(error)
            }
        },
        async updateBurguer(event, id) {
            try {
                const option = event.target.value;
                const dataJson = JSON.stringify({ status: option });
                const req = await axios.patch(`http://localhost:3000/burgers/${id}`, dataJson);
                const res = req.data
                console.log(res)
                this.msg = `Pedido Nº ${res.id} atualizado para '${res.status}' com sucesso!`
                setTimeout(() => {
                    this.msg = ''
                }, 5000);
            } catch (error) {
                console.error(error)
            }
        },
        abrirModal(id) {
            this.aberto = true
            this.burguer_id = id
        }
    },
    mounted() {
        this.getPedidos()
    },
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
    width: 19%;
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
    border: 1px solid #333;
    @apply py-[12px] px-[6px] mr-12 rounded-md
}

.delete-btn {
    border: 2px solid #222;
    font-size: 16px;
    margin: 0 auto;
    margin-top: 5px;
    transition: 0.5s;
    @apply bg-[#222] text-[#FCBA03] font-bold p-[10px] cursor-pointer rounded-md
}

.delete-btn:hover {
    @apply bg-transparent text-[#333]
}
</style>