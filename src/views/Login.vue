<template>
  <div>
    <h1>Registro</h1>
    <p>Email: <input v-model="email"></p>
    <p>Senha: <input type="password" v-model="senha"></p>
    <p>
      <button v-on:click="cadastrar()">Cadastrar</button>
      <button v-on:click="entrar()">Entrar</button>
    </p>
  </div>
</template>

<script>
import supabase from '../supabase-client';
import router from '../router';

export default {
  name: 'Login',
  data: () => {
    return {
      email: '',
      senha: ''
    };
  },
  methods: {
    async cadastrar(){
      try{
        const res = await supabase.auth.signUp({
          email: this.email,
          password: this.senha
        });

        if(res.error){
          console.log(res.error.message);
        } else {
          router.push('/');
        }
      } catch(err){
        console.error(err);
        alert('Não foi possivel cadastrar');
      }
    },
    async entrar(){
      try{
        const res = await supabase.auth.signIn({
          email: this.email,
          password: this.senha
        });

        if(res.error){
          console.log(res.error);
        } else {
          router.push('/');
        }
      } catch(err){
        console.error(err);
        alert('Não foi possivel cadastrar');
      }
    }
  },
  components: {}
}
</script>
