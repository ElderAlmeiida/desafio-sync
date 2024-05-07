<template>
  <div>
    <!-- Botão para abrir o formulário -->
    <div class="text-center">
      <button @click="exibirFormulario" class="btn btn-primary  m-2  " style="background-color: #475DCE; font-weight: bold; border-radius: 0; font-size: 16px; width: 242px; box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);">Adicionar Usuário</button>
    
      
    </div>

    <!-- Popup do formulário -->
    <transition name="fade">
      <div v-if="mostrarPopup" class="popup">
        <div class="popup-content" style="padding: 2rem; position: relative;">
          <span class="close " @click="fecharFormulario" style="position: absolute; top: -59px; right: -24px; cursor: pointer; font-size: 70px; color: red;">&times;</span>
          <h2 class="mb-5 text-center" style="font-family: 'Roboto', sans-serif; font-weight: bold;"></h2>
          <form @submit.prevent="atualizarInfo" class="row g-4">
            
            <div class="col-md-6">
              <input type="text" id="nome" v-model="novoNome" class="form-control" placeholder="Nome" required>
            </div>
            <div class="col-md-6">
                <input type="text" id="idade" v-model="novaIdade" class="form-control" placeholder="Idade" required pattern="[0-9]*">
            </div>
            <div class="col-md-4">
              <input type="text" id="rua" v-model="novaRua" class="form-control" placeholder="Rua" required>
            </div>
            <div class="col-md-4">
              <input type="text" id="bairro" v-model="novoBairro" class="form-control" placeholder="Bairro" required>
            </div>
            <div class="col-md-4">
              <input type="text" id="estado" v-model="novoEstado" class="form-control" placeholder="Estado" required>
            </div>
            <div class="col-12">
              <textarea id="biografia" v-model="novaBiografia" class="form-control" placeholder="Biografia" required></textarea>
            </div>
            <div class="col-md-6">
              <input type="file" id="imagem" @change="handleImagemUpload" class="form-control" accept="image/*">
              <div v-if="imagemSelecionada" class="alert alert-success mt-2" role="alert">
                Imagem selecionada!
              </div>
            </div>
            <div class="col-12 text-center">
              <button type="submit" class="btn btn-primary" style="background-color: #475DCE; font-weight: bold; border-radius: 0; font-size: 16px; width: 242px; box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);">Novo Usuario</button>
            </div>
          </form>
        </div>
      </div>
    </transition>
  </div>
</template>

<script>

export default {
  data() {
    return {
      novoNome: '',
      novaIdade: 0,
      novaRua: '',
      novoBairro: '',
      novoEstado: '',
      novaBiografia: '',
      imagemSelecionada: null,
      mostrarPopup: false
    };
  },
  methods: {
    exibirFormulario() {
      this.mostrarPopup = true;
    },
    fecharFormulario() {
      this.mostrarPopup = false;
      // Resetar o formulário ao fechar
      this.resetarFormulario();
    },
    handleImagemUpload(event) {
      this.imagemSelecionada = event.target.files[0];
      // Exibir alerta quando a imagem for selecionada
      if (this.imagemSelecionada) {
        
      }
    },
    atualizarInfo() {
      this.$emit('atualizar-info', {
        nome: this.novoNome,
        idade: this.novaIdade,
        rua: this.novaRua,
        bairro: this.novoBairro,
        estado: this.novoEstado,
        biografia: this.novaBiografia,
        imagem: this.imagemSelecionada
      });
      this.fecharFormulario();
    },
    resetarFormulario() {
      this.novoNome = '';
      this.novaIdade = 0;
      this.novaRua = '';
      this.novoBairro = '';
      this.novoEstado = '';
      this.novaBiografia = '';
      this.imagemSelecionada = null;
    },
    salvarInformacoes() {
  // Recuperar os dados existentes do localStorage
      const usuariosSalvos = JSON.parse(localStorage.getItem('usuarios')) || [];

      // Adicionar o novo usuário ao array de usuários
      const novoUsuario = {
        nome: this.novoNome,
        idade: this.novaIdade,
        rua: this.novaRua,
        bairro: this.novoBairro,
        estado: this.novoEstado,
        biografia: this.novaBiografia,
        imagem: this.imagemSelecionada
      };
      usuariosSalvos.push(novoUsuario);

      // Salvar o array atualizado de volta no localStorage
      localStorage.setItem('usuarios', JSON.stringify(usuariosSalvos));

      // Fechar o formulário após salvar
      this.fecharFormulario();
    }
  }
};
</script>

<style scoped>
/* Estilo para o popup */
.popup {
  display: flex;
  justify-content: center;
  align-items: center;
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background-color: rgba(0, 0, 0, 0.5);
}

.popup-content {
  background-color: white;
  border-radius: 5px;
  width:700px;
}

.fade-enter-active,
.fade-leave-active {
  transition: opacity 0.5s;
}
.fade-enter, .fade-leave-to  {
  opacity: 0;
}

body {
  font-family: 'Roboto', sans-serif;
}

@media (max-width: 768px) {
  .popup-content{
    margin:1rem;
  }
}
</style>







  
  



	