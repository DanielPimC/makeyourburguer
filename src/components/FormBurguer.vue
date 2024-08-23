<template>
    <div>
        <div>
            <form id="burguerform" @submit.prevent="createBurguer"> 
                <h1 class="monte-burguer">MONTE SEU BURGUER:</h1>
                <Message :msg="msg" v-show="msg" />
                <div class="input-container">
                    <label for="nome">Nome do cliente: </label>
                    <input type="text" name="nome" placeholder="Nome" v-model="nome">
                </div>
                <div class="input-container">
                    <label for="pao">Escolha o seu pão: </label>
                    <select name="pao" id="pao" v-model="pao">
                        <option value="">Selecione o pão</option>
                        <option v-for="pao in paes" :key="pao.id" :value="pao.tipo">{{ pao.tipo }}</option>
                    </select>
                </div>
                <div class="input-container">
                    <label for="carne">Escolha a carne: </label>
                    <select name="carne" id="carne" v-model="carne">
                        <option value="">Selecione a carne</option>
                        <option v-for="carne in carnes" :key="carne.id" :value="carne.tipo">{{ carne.tipo }}</option>
                    </select>
                </div>
                <div id="opcionais-container" class="input-container">
                    <label for="opcionais" class="opcionais-title">Escolha os opcionais: </label>
                    <div class="checkbox-container" v-for="opcional in opcionaisdata" :key="opcional.id" >
                        <input type="checkbox" name="opcionais" v-model="opcionais" :value="opcional.tipo">
                        <span>{{ opcional.tipo }}</span>
                        
                    </div>
                </div>
                <div class="input-container">
                    <input type="submit" class="submit-btn" value="Confirmar pedido">
                </div>
            </form>
        </div>
    </div>
</template>

<script>
import axios from 'axios'
import Message from './Message.vue'

export default {
    name: 'FormBurguer',
    components: {
        Message
    },
    data() {
        return {
            paes: null,
            carnes: null,
            opcionaisdata: null,
            nome: null,
            pao: null,
            carne: null,
            opcionais: [],
            msg: null
        }
    },
    methods: {
        async getIngredients() {
            try {
                const req = await axios.get('http://localhost:3000/ingredientes')
                this.paes = req.data.paes
                this.carnes = req.data.carnes
                this.opcionaisdata = req.data.opcionais
            }catch(error){
                console.error(error)
            }
        },
        async createBurguer(e) {
            const data = {
                nome: this.nome,
                pao: this.pao,
                carne: this.carne,
                opcionais: Array.from(this.opcionais),
                status: 'Solicitado'
            }

            const req = await axios.post('http://localhost:3000/burgers', data)
            const res = req.data
            console.log(res)
            this.nome = ""
            this.paes = ""
            this.carnes = ""
            this.opcionais = ""
            this.msg = `Pedido Nº ${res.id} feito com sucesso!`
            
            setTimeout(() => {
                 this.msg = ''
            }, 5000);
        }
    },
    mounted() {
        this.getIngredients()
    }
}
</script>

<style scoped>

    .monte-burguer {
        @apply text-[30px] mb-10 font-bold
    }
    #burger-form {
        max-width: 400px;
        margin: 0 auto;
    }

    .input-container {
        display: flex;
        flex-direction: column;
        align-items: center;
        margin-bottom: 20px;
    }

    label{
        border-left: 4px solid #FCBA03;
        @apply font-bold mb-[15px] text-[#222] px-[5px] py-[10px]
    }

    input, select {
        align-items: center;
        text-align: center;
        border: 1px solid black;
        @apply px-[5px] py-[10px] w-[300px]
    }

    #opcionais-container {
        display: flex;
        flex-direction: row;
        flex-wrap: wrap;
        flex-direction: column;
        margin-bottom: 20px;
    }

    .opcionais-title {
        @apply w-[100px]
    }

    .checkbox-container {
        @apply flex text-start w-[50%] mb-[20px]
    }

    .checkbox-container span,
    .checkbox-container input {
        @apply w-auto
    }

    .checkbox-container span {
        @apply ml-[6px] font-bold
    }

    .submit-btn {
        border: 2px solid #222;
        font-size: 16px;
        margin: 0 auto;
        @apply bg-[#222] text-[#FCBA03] font-bold p-[10px] cursor-pointer rounded-lg duration-500
    }

    .submit-btn:hover {
        @apply bg-transparent text-[#222]
    }
</style>