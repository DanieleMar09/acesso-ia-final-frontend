<template>
  <div class="container mt-5">
    <div class="row justify-content-center">
      <div class="col-md-6">
        <div class="card shadow p-4">
          <h3 class="text-center mb-4">Recuperar Senha</h3>

          <form @submit.prevent="handleSubmit">
            <div class="form-group">
              <label for="email">Informe seu email</label>
              <input
                v-model="email"
                type="email"
                class="form-control"
                placeholder="Digite seu email"
                :disabled="emailEncontrado"
                required
              />
            </div>

           
            <div v-if="emailEncontrado">
              <div class="form-group mt-3">
                <label for="senha">Nova Senha</label>
                <input
                  v-model="senha"
                  type="password"
                  class="form-control"
                  placeholder="Digite a nova senha"
                  required
                />
              </div>
              <div class="form-group mt-3">
                <label for="confirmarSenha">Confirmar Nova Senha</label>
                <input
                  v-model="confirmarSenha"
                  type="password"
                  class="form-control"
                  placeholder="Confirme a nova senha"
                  required
                />
              </div>
            </div>

            <div class="text-center mt-4">
              <button class="btn btn-primary" type="submit">
                <i class="fa" :class="emailEncontrado ? 'fa-envelope' : 'fa-envelope'"></i>
                {{ emailEncontrado ? 'Redefinir Senha' : 'Recuperar Senha' }}
              </button>
              <router-link to="/login" class="btn btn-link d-block mt-2">
                <i class="fa fa-arrow-left"></i> Voltar para o Login
              </router-link>
            </div>
          </form>
        </div>
      </div>
    </div>
  </div>
</template>

  
<script>
import axios from "axios";
import Swal from "sweetalert2";

export default {
  data() {
    return {
      email: "",
      senha: "",
      confirmarSenha: "",
      usuarioId: null,
      emailEncontrado: false,
    };
  },
  methods: {
    async handleSubmit() {
      if (!this.emailEncontrado) {
      
        if (!this.email) {
          Swal.fire({
            icon: "warning",
            title: "Campo obrigatório",
            text: "Por favor, informe o email.",
          });
          return;
        }

        try {
          const response = await axios.get(
            `https://localhost:7263/api/v1/login/buscar-email?email=${this.email}`
          );
          this.usuarioId = response.data.id;
          this.emailEncontrado = true;

          Swal.fire({
            icon: "info",
            title: "Email encontrado!",
            text: "Agora digite e confirme a nova senha.",
          });

        } catch (error) {
          const msg = error.response?.data || "Erro ao buscar o usuário";
          Swal.fire({
            icon: "error",
            title: "Email não encontrado",
            text: typeof msg === "string"
              ? msg
              : msg.title || msg.errors?.Usuario?.[0] || "O email informado não está cadastrado.",
          });
        }

      } else {
        // Passo 2: Redefinir senha
        if (!this.senha || !this.confirmarSenha) {
          Swal.fire({
            icon: "warning",
            title: "Campos obrigatórios",
            text: "Por favor, preencha os campos de senha.",
          });
          return;
        }

        if (this.senha !== this.confirmarSenha) {
          Swal.fire({
            icon: "error",
            title: "Senhas diferentes",
          text: "A confirmação não corresponde à nova senha.",
          });
          return;
        }

        try {
          await axios.put(
            `https://localhost:7263/api/v1/login/resetar-senha/${this.usuarioId}`,
            { novaSenha: this.senha }
          );

          Swal.fire({
            icon: "success",
            title: "Senha redefinida!",
            text: "Uma nova senha foi gerada com sucesso.",
          });

          this.$router.push("/login");

        } catch (error) {
          Swal.fire({
            icon: "error",
            title: "Erro ao redefinir",
            text: "Não foi possível redefinir a senha.",
          });
        }
      }
    },
  },
};
</script>
