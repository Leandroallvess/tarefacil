<template>
    <div class="m-4">
        <form @submit.prevent="salvarTarefa">
            <div class="mb-3">
                <label class="form-label">Nova Tarefa</label>
                <div class="d-flex flex-row">
                    <input v-model="tarefa" type="text" class="form-control me-2" placeholder="ex: fazer dever de casa">
                    <button type="submit" class="btn btn-primary">Salvar</button>
                </div>
            </div>
        </form>
    </div>
    <div class="m-4">
        <h3>Tarefas para fazer</h3>

        <p v-if="tarefas.length == 0">Voce não tem nenhuma tarefa para fazer ;)</p>

        <ul class="list-group mb-1" v-for="(tarefa, i) in tarefas" :key="i">
            <li class="list-group-item d-flex justify-content-between align-items-center">
                {{ tarefa }}
                <div>
                    <button 
                        type="button" 
                        title="Marcar tarefa como como concluida" 
                        class="btn btn-success m-1"
                        @click="marcarConcluida(i)"
                    >
                        <i class="fas fa-check"></i>
                    </button>
                    <button 
                        type="button" 
                        title="excluir tarefa" 
                        class="btn btn-danger"
                        @click="removerTarefa(i)"
                    >
                        <i class="fas fa-close"></i>
                    </button>
                </div>
            </li>
        </ul>
    </div>
    <div class="m-4">
        <h3>Tarefas concluidas</h3>

        <p v-if="tarefasConcluidas.length == 0">Voce ainda não concluiu nenhuma tarefa :(</p>

        <ul class="list-group m-1" v-for="(tarefa, i) in tarefasConcluidas" :key="i">
            <li class="list-group-item d-flex justify-content-between align-items-center">
                {{ tarefa }}
                <button 
                    type="button" 
                    title="refazer tarefa" 
                    class="btn btn-primary"
                    @click="refazerTarefa(i)"
                >
                    <i class="fas fa-recycle"></i>
                </button>
            </li>
        </ul>
    </div>

    <div class="m-4">
        <h3>Tarefas Removidas</h3>

        <p v-if="tarefasRemovidas.length == 0">Nenhuma tarefa foi removida até o momento.</p>

        <ul class="list-group m-1" v-for="(tarefa, i) in tarefasRemovidas" :key="i">
            <li class="list-group-item d-flex justify-content-between align-items-center">
                {{ tarefa }}
                <button 
                    type="button" 
                    title="restaurar tarefa" 
                    class="btn btn-warning"
                    @click="restaurarTarefa(i)"
                >
                    <i class="fas fa-reply"></i>
                </button>
            </li>
        </ul>
    </div>
</template>

<script setup>
import { ref, reactive } from 'vue'

const tarefa = ref()
const tarefas = reactive([])
const tarefasConcluidas = reactive([])
const tarefasRemovidas = reactive([])

function salvarTarefa() {
    tarefas.unshift(tarefa.value)
    tarefa.value = ''
}

function marcarConcluida(index){
    const tarefasConcluida = tarefas.splice(index, 1)
    tarefasConcluidas.unshift(...tarefasConcluida)
}

function removerTarefa(index){
    const tarefaRemovida = tarefas.splice(index, 1)
    tarefasRemovidas.unshift(...tarefaRemovida)
}

function refazerTarefa(index){
    const tarefa = tarefasConcluidas.splice(index, 1)
    tarefas.unshift(...tarefa)
}

function restaurarTarefa(index){
    const tarefa = tarefasRemovidas.splice(index, 1)
    tarefas.unshift(...tarefa)
}

</script>