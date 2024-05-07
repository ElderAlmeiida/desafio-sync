<template>
  <div class="container mt-4">
    <div class="row">
      <div class="col-md-4 mt-5">
        <div class="info text-center">
          <img :src="imagemPerfil" alt="Imagem de Perfil" class="img-fluid rounded-circle shadow-sm p-2 mb-5 bg-white rounded" style="width: 250px; height: 250px; object-fit: cover;">
        </div>
      </div>
      <div class="col-md-8 align-items-center mt-5">
        <div class="shadow-sm p-3 mb-5 bg-white rounded">
          <p><strong>Nome:</strong> {{ nome }}</p>
          <p><strong>Idade:</strong> {{ idade }}</p>
          <p><strong>Rua:</strong> {{ rua }}</p>
          <p><strong>Bairro:</strong> {{ bairro }}</p>
          <p><strong>Estado:</strong> {{ estado }}</p>
          <p><strong>Biografia:</strong> {{ biografia }}</p>
        </div>
        <hr>
        <Formulario @atualizar-info="atualizarInfoUsuario" />
      </div>
    </div>
  
    <!-- Tabela de usuários -->
    <div class="row mt-5" v-if="usuarios.length > 0">
      <div class="col">
        <h3>Usuários Salvos</h3>
        <div class="table-responsive">
          <table class="table text-center">
            <thead>
              <tr>
                <th>Imagem</th>
                <th>Nome</th>
                <th>Idade</th>
                <th>Rua</th>
                <th>Bairro</th>
                <th>Estado</th>
                <th>Ações</th>
              </tr>
            </thead>
            <tbody>
              <tr v-for="(usuario, index) in usuarios" :key="index" class="shadow-sm p-3 mb-5 bg-white rounded" :class="{ 'table-primary': usuario === usuarioSelecionado }">
                <td :style="{ transform: usuario === usuarioSelecionado ? 'scale(1.1)' : 'none' }"><img :src="usuario.imagemPerfil" alt="Imagem de Perfil" class="img-fluid rounded-circle shadow-sm p-2 bg-white rounded" style="width: 50px; height: 50px; object-fit: cover; vertical-align: middle;"></td>
                <td>{{ usuario.nome }}</td>
                <td>{{ usuario.idade }}</td>
                <td>{{ usuario.rua }}</td>
                <td>{{ usuario.bairro }}</td>
                <td>{{ usuario.estado }}</td>
                
                <td>
                  <div class="d-flex justify-content-center">
                    <button @click="selecionarUsuario(usuario)" class="btn btn-primary" style="margin-right: 0.5rem;">
                      <img src="../assets/Edit.svg" alt="Editar" style="width: 1rem; height: 1rem;">
                    </button>
                    <button @click="excluirUsuario(index)" class="btn btn-danger" style="margin-left: 0.5rem;">
                      <img src="../assets/Close-White.svg" alt="Excluir" style="width: 1rem; height: 1rem;">
                    </button>
                  </div>
                </td>
              </tr>
            </tbody>
          </table>
        </div>
      </div>
    </div>
    
    <!-- Formulário de Edição -->
    <div v-if="usuarioSelecionado" class="row mt-5">
      <div class="col-md-8 offset-md-2">
        <FormularioEdicao @atualizar-info="salvarEdicaoUsuario" :dados-usuario="usuarioSelecionado" />
      </div>
    </div>
  </div>
</template>

<script>
import minhaImagemDePerfil from '../assets/user.svg';
import Formulario from './Formulario.vue';
import FormularioEdicao from './FormularioEdicao.vue';

export default {
  components: {
    Formulario,
    FormularioEdicao
  },
  data() {
    return {
      nome: 'Fulano',
      idade: 2024,
      rua: 'exemplo',
      bairro: 'exemplo',
      estado: 'exemplo',
      biografia: 'nenhum usuário adicionado, experimente adicionar',
      imagemPerfil: minhaImagemDePerfil,
      usuarios: [], 
      usuarioSelecionado: null
    };
  },
  created() {
    const usuariosSalvos = JSON.parse(localStorage.getItem('usuarios'));
    if (usuariosSalvos) {
      this.usuarios = usuariosSalvos;
    }
  },
  methods: {
    atualizarInfoUsuario(dadosAtualizados) {
      this.nome = dadosAtualizados.nome;
      this.idade = dadosAtualizados.idade;
      this.rua = dadosAtualizados.rua;
      this.bairro = dadosAtualizados.bairro;
      this.estado = dadosAtualizados.estado;
      this.biografia = dadosAtualizados.biografia;
      this.imagemPerfil = URL.createObjectURL(dadosAtualizados.imagem);

      const novoUsuario = {
        nome: dadosAtualizados.nome,
        idade: dadosAtualizados.idade,
        rua: dadosAtualizados.rua,
        bairro: dadosAtualizados.bairro,
        estado: dadosAtualizados.estado,
        biografia: dadosAtualizados.biografia,
        imagemPerfil: URL.createObjectURL(dadosAtualizados.imagem)
      };
      this.usuarios.push(novoUsuario);
      localStorage.setItem('usuarios', JSON.stringify(this.usuarios));
    },
    selecionarUsuario(usuario) {
      this.usuarioSelecionado = usuario;
      this.nome = usuario.nome;
      this.idade = usuario.idade;
      this.rua = usuario.rua;
      this.bairro = usuario.bairro;
      this.estado = usuario.estado;
      this.biografia = usuario.biografia;
      this.imagemPerfil = usuario.imagemPerfil;
    },
    editarUsuario(usuario) {
      // Remova esta função, pois não é mais necessária aqui
    },
    salvarEdicaoUsuario(dadosAtualizados) {
      this.usuarioSelecionado.nome = dadosAtualizados.nome;
      this.usuarioSelecionado.idade = dadosAtualizados.idade;
      this.usuarioSelecionado.rua = dadosAtualizados.rua;
      this.usuarioSelecionado.bairro = dadosAtualizados.bairro;
      this.usuarioSelecionado.estado = dadosAtualizados.estado;
      this.usuarioSelecionado.biografia = dadosAtualizados.biografia;
      this.usuarioSelecionado.imagemPerfil = dadosAtualizados.imagem;
      
      localStorage.setItem('usuarios', JSON.stringify(this.usuarios));
    },
    excluirUsuario(index) {
      this.usuarios.splice(index, 1); 
      localStorage.setItem('usuarios', JSON.stringify(this.usuarios)); 
      if (this.usuarioSelecionado === this.usuarios[index]) {
        this.usuarioSelecionado = null;
      }
    }
  }
};
</script>

<style scoped>
.col-md-4 {
}
.table tbody tr {
  vertical-align: middle;
}

/* Estilos para dispositivos móveis */
@media (max-width: 767px) {
  .table td:nth-child(3),
  .table th:nth-child(3),
  .table td:nth-child(4),
  .table th:nth-child(4),
  .table td:nth-child(5),
  .table th:nth-child(5),
  .table td:nth-child(6),
  .table th:nth-child(6) {
    display: none !important; 
  }
}
</style>

