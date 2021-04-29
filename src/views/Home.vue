<template>
  <div class="home">
    <h1>Tarefas</h1>
    <input v-model="titulo">
    <button v-on:click="adicionarTarefa()">
      Nova tarefa
    </button>
    <ul>
      <li v-for="tarefa in tarefas" :key="tarefa.id">
        {{tarefa.titulo}}
        <button v-on:click="concluirTarefa(tarefa)">X</button>
      </li>
    </ul>
  </div>
</template>

<script>
// @ is an alias to /src
import supabase from '../supabase-client';

export default {
  name: 'Home',
  data: () => {
    return {
      titulo: '',
      tarefas: []
    };
  },
  methods: {
    async adicionarTarefa(){
      // this.tarefas.push({
      //   id: 4,
      //   titulo: this.titulo
      // });
      // this.titulo='';
      try{
        const res = await supabase.from('tarefas')
          .insert({
            user_id: supabase.auth.user().id,
            titulo: this.titulo
          });

        if(res.error){
          alert(res.error.message);
        } else {
          this.titulo = '';
          this.carregarTarefas();
        }

      } catch (err){
        console.error(err);
        alert('Nao foi possivel cadastrar tarefa');
      }
    },
    async concluirTarefa(tarefa){
      // const idx = this.tarefas.indexOf(tarefa);
      // this.tarefas.splice(idx, 1);
      // this.tarefas = this.tarefas.filter(x => x !== tarefa);
      try{

        const res = await supabase.from('tarefas')
          .update({
            concluida: true
          })
          .eq('id', tarefa.id);

        if(res.error){
          alert(res.error.message);
        } else {
          this.carregarTarefas();
        }

      } catch (err){
        console.error(err);
        alert('Nao foi possivel atualizar tarefa');
      }
    },
    async carregarTarefas(){
      try{
        const res = await supabase.from ('tarefas')
          .select('id, titulo')
          .filter('concluida', 'eq', false);

        if(res.error){
          alert(res.error.message);
        } else {
          this.tarefas = res.data;
        }

      } catch (err){
        console.error(err);
        alert('Nao foi possivel carregar as tarefas');
      }
    },
  },
  mounted(){
    this.carregarTarefas();
  },
  components: {
  }
}
</script>
