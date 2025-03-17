<template>
    <div>
        <h2>Gernciamento de usuarios </h2>

        <!-- Botao para criar novo -->
         <div class="mb-3">
            <router-link to="/usuario/cadastrar" class="btn btn-success">
                <i class="fa fa-plus">Adicionar novo</i>
            </router-link>
         </div>

         <div  v-if="carregando" class="text-center">
            <i class="fa fa-spinner fa-spin"></i>Carregando....
         </div>

         <!-- Tabela de listagem -->
          <table  v-if="!carregando" class="table table-striped">
            <thead class="thead-dark">
                <tr>
                    <th>Id</th>
                    <th>Nome</th>
                    <th>Email</th>
                    <th>CPF</th>
                    <th>Status</th>
                    <th>Opções</th>
                </tr>
            </thead>
            <tbody>
                <tr v-for="usuario in usuarios" :key="usuario.id">
                <td>{{ usuario.id }}</td>
                <td>{{ usuario.nome }}</td>
                <td>{{ usuario.email }}</td>
                <td>{{ usuario.cpf }}</td>
                <td>{{ usuario.status }}</td>

            
                <td>
                    <button class="btn btn-primary btn-sm">
                        <i class="fa fa-edit"></i>Editar
                    </button>

                    <button class="btn btn-danger btn-sm ml-2" >
                        <i class="fa fa-trash"></i>Excluir
                    </button>
                </td>
            </tr>

            </tbody>
          </table>
    </div>



</template>
<script>
import axios from 'axios';
export default{
    data(){
        return{
            usuarios:[],
            carregando:false,
        };
    },
    mounted(){
this.carregarUsuarios();
    },
    methods:{
        async carregarUsuarios()
        {
            this.carregando = true;
        
            try{
                const response = await axios.get("https://localhost:7263/api/v1/usuarios/listar-todos");
                setTimeout(() =>{
                    this.usuarios = response.data;
                   this.carregando = false;

                }, 3000)
         

            }catch(error){
                console.log(error);
                this.carregando =false;

            }

        }

    }
}
</script>