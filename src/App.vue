<script setup>
  import { reactive } from 'vue';
  import Cabecalho from './components/Cabecalho.vue';
  import Formulario from './components/Formulario.vue';
  import Tasklist from './components/Tasklist.vue';

const estado = reactive({
  filtro: 'todas',
  tarefaTemp: '',
  tarefas: [
    {
    titulo: "estudar ES6",
    finalizada: false,
    },
    {
    titulo: "estudar sass",
    finalizada: false,
    },
    {
    titulo: "ir pra academia",
    finalizada: true,
    },
]
})

const getTarefasPendentes = () => {
  return estado.tarefas.filter(tarefa => !tarefa.finalizada);
}

const getTarefasFinalizadas = () => {
  return estado.tarefas.filter(tarefa => tarefa.finalizada);
}

const getTarefasFiltradas = () => {
  const { filtro } = estado;

  switch(filtro) {
    case 'pendentes':
      return  getTarefasPendentes();
    case 'finalizadas':
      return getTarefasFinalizadas();
    default:
      return estado.tarefas;
  }
}

const cadastraTarefa = () => {
  const tarefaNova = {
    titulo: estado.tarefaTemp,
    finalizada: false,
  }
  estado.tarefas.push(tarefaNova);
  estado.tarefaTemp = '';
}

</script>

<template>
  <div class="container">
    <Cabecalho :tarefas-pendentes="getTarefasPendentes().length" />
    <Formulario :trocar-filtro="evento => estado.filtro = evento.target.value" 
                :tarefa-temp="estado.tarefaTemporaria" 
                :edita-tarefa-temp="evento => estado.tarefaTemporaria = evento.target.value" 
                :cadastra-tarefa="cadastraTarefa" />

    <Tasklist :tarefas="getTarefasFiltradas()" />
  </div>
</template>