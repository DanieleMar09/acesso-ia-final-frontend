<template>
  <div class="login-container">
   <div class="login">
    <img src="@/assets/logo.png" class="logo"/>
    <h3 class="text-center"> Acesse sua conta </h3>
    <form @submit.prevent ="login">
      <div class="form-group">
      <label>Login</label>
      <input type="text"  v-model="dadosLogin.login" class="form-control" required>
      </div>
     
    <div class="form-group">
      <label>Senha</label>
      <input type="password" v-model="dadosLogin.senha" class="form-control" required>
      </div>

        <div class="form-group">
      <button type="submit" class="btn btn-primary btn-block">
      Acessar
      </button>
      <p v-if="errorAlerta" class="text-danger text-center">
      {{errorAlerta}}
      </div>


      </form>
      </div>
    </div>

</template>

<script>
import axios from 'axios'
export default {
  data() {
    return {
      dadosLogin:{
        login :'',
        senha:''
      },
      errorAlerta: ''
    };
  },
  methods: {
    async Processarlogin(){
      try{
        const response = await axios.post('https://localhost:7263/api/v1/login/autenticar',this.dadosLogin);
        const resultado = response.data;
      }catch(error){
        this.errorAlerta = 'Login ou Senha Inválidos';

      }
    }
  }
};
</script>
import axios from 'axios'

<style scoped>

.login-container{
  display:flex;
  justify-content:center;
  align-items: center;
  height:100vh;
  background-color: #f5f5f5;
}

.login
{
  background: #white;
  padding:30px;
  border-radius:10px;
  box-shadow: 0 0 10px (0,0,0,0.1);
  width:350px;
   text-align: center;
}

.logo
{
  width:150px;
  margim-bottom:20px;

}

</style>