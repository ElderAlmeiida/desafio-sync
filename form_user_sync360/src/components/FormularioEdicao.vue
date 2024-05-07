<template>
  <div>
    <div class="modal" v-if="mostrarPopup">
      <div class="modal-content">
        <span class="close" @click="fecharPopup" style="position: absolute; top: -59px; right: -24px; cursor: pointer; font-size: 70px; color: red;">&times;</span>
        <div>
          <h3 style="margin-bottom: 1.5rem; font-weight: bold; text-align: center;">Editar Usuário</h3>
          <form @submit.prevent="salvarEdicao">
            <div class="mb-3">
              <input type="text" class="form-control" placeholder="Nome" v-model="usuario.nome">
            </div>
            <div class="mb-3">
              <input type="number" class="form-control" placeholder="Idade" v-model="usuario.idade">
            </div>
            <div class="mb-3">
              <input type="text" class="form-control" placeholder="Rua" v-model="usuario.rua">
            </div>
            <div class="mb-3">
              <input type="text" class="form-control" placeholder="Bairro" v-model="usuario.bairro">
            </div>
            <div class="mb-3">
              <input type="text" class="form-control" placeholder="Estado" v-model="usuario.estado">
            </div>
            <div class="mb-3">
              <textarea class="form-control" placeholder="Biografia" rows="3" v-model="usuario.biografia"></textarea>
            </div>
            <div class="mb-3">
              <input type="file" class="form-control" @change="processarImagem" accept="image/*">
            </div>
            <button type="submit" class="btn btn-primary" style="background-color: #475DCE; font-weight: bold; border-radius: 0; font-size: 16px; width: 242px; box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);">Salvar</button>
          </form>
        </div>
      </div>
    </div>
    <div class="overlay" @click="fecharPopup" v-if="mostrarPopup"></div>
  </div>
</template>

<script>
export default {
  props: {
    dadosUsuario: {
      type: Object,
      required: true
    }
  },
  data() {
    return {
      usuario: {
        ...this.dadosUsuario 
      },
      mostrarPopup: true 
    };
  },
  methods: {
    salvarEdicao() {
      this.$emit('atualizar-info', this.usuario); 
      this.fecharPopup();
    },
    processarImagem(event) {
      const file = event.target.files[0];
      if (file) {
        const reader = new FileReader();
        reader.readAsDataURL(file);
        reader.onload = () => {
          this.usuario.imagem = reader.result;
        };
      }
    },
    fecharPopup() {
      this.mostrarPopup = false;
    }
  }
};
</script>

<style scoped>
.modal {
  display: flex;
  justify-content: center;
  align-items: center;
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  z-index: 1000;
}

.modal-content {
  background-color: #fefefe;
  border-radius: 5px;
  padding: 20px;
  box-shadow: 0 4px 8px 0 rgba(0, 0, 0, 0.2);
  width: 70%;
  max-width: 600px;
  position: relative;
}

.close {
  color: #aaa;
  position: absolute;
  top: 10px;
  right: 10px;
  font-size: 28px;
  font-weight: bold;
  cursor: pointer;
}

.close:hover,
.close:focus {
  color: black;
  text-decoration: none;
}

.overlay {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background-color: rgba(0, 0, 0, 0.5);
}
</style>

  
  
  