<template>
  <div class="container">
    <div class="left-panel">
      <h1 class="titulo">
        <span class="text-papelaria">Papelaria</span><span class="text-lale">Lalê</span>
      </h1>
    </div>
    <div class="right-panel">
      <h3 class="form-title">Registre-se</h3>
      <form class="cadastro-form" @submit.prevent="submitForm">
        <label for="nomecompleto">Nome completo</label>
        <input type="text" id="nomecompleto" v-model="form.nomeCompleto" placeholder="Nome completo" class="form-element">

        <label for="email">E-mail</label>
        <input type="email" id="email" v-model="form.email" placeholder="Seu e-mail" class="form-element">

        <label for="password">Digite sua senha</label>
        <input type="password" id="password" v-model="form.password" placeholder="Digite sua senha" class="form-element">

        <label for="confirm_password">Confirme sua senha</label>
        <input type="password" id="confirm_password" v-model="form.confirmPassword" placeholder="Confirme sua senha" class="form-element">

        <button type="submit" class="btn-cadastrar form-element">Cadastrar</button>

        <div class="separator"></div>
        <p class="Login-page">Já tem uma conta? 
          <router-link to="/login" class="entrar-link">Entrar</router-link>
        </p>
      </form>
    </div>
  </div>

  <SuccessMessageModal
      v-if="isModalVisible"
      :show="isModalVisible"
      message="Cadastro efetuado com sucesso!!"
      @close="isModalVisible = false"
    />
</template>

<script>
import axios from 'axios';
import SuccessMessageModal from '../components/SuccessMessageModal.vue';

export default {
  name: 'RegisterForm',

  components: {
    SuccessMessageModal
  },

  data() {
    return {
      isModalVisible: false,
      form: {
        nomeCompleto: '',
        email: '',
        password: '',
        confirmPassword: '',
      },
    };
  },
  methods: {
    sleep(ms) {
      return new Promise(resolve => setTimeout(resolve, ms));
    },

    async submitForm() {
      if (this.form.password !== this.form.confirmPassword) {
        alert("As senhas não coincidem!");
        return;
      }
      console.log("Formulário enviado:", this.form);
      try {
        await axios.post('http://localhost:9000/api/token/', {
          name: this.form.nomeCompleto,
          email: this.form.email,
          password: this.form.password
        });

        this.isModalVisible = true
        await this.sleep(2000);
        
        this.$router.push({ name: 'Login' });
      } catch (err) {
        this.error = 'Invalid credentials';
      }
    }
  }
}
</script>

<style>

/* Estilos gerais */
body {
  background-color: #fffafa;
}

.container {
  display: flex;
  height: 100vh;
}

/* Painel Esquerdo */
.left-panel {
  flex: 3;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  text-align: center;
}

.left-panel h1 {
  font-size: 5vw; 
  color: #4B0082;
  margin: 0;
}

.text-papelaria {
  color: #cb6ce6;
}

.left-panel p {
  color: #4B0082;
  font-size: 14px;
}

.papelaria-logo {
  width: 50px;
  height: auto;
  position: absolute;
  bottom: 80px;
  left: 50%;
  transform: translateX(-50%);
  border-radius: 10%;
}

/* Painel Direito */
.right-panel {
  flex: 1;
  background-color:  #cb6ce6;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  padding: 20px;
  position: relative;
}

.right-panel h3 {
  color: #fffafa;
  margin-top: 0;
  margin-bottom: 40px;
  font-size: 24px;
  text-align: center;
}

.cadastro-form {
  width: 75%;
  max-width: 400px;
}

/* Estilo comum para inputs, select e botão */
.cadastro-form .form-element {
  width: 100%;
  padding: 10px;
  margin-top: 5px;
  border: none;
  border-radius: 4px;
  margin-bottom: 10px;
  box-sizing: border-box;
}

.cadastro-form input::placeholder {
  color: #666;
}

/* Estilo dos labels */
.cadastro-form label {
  color: white;
  font-size: 14px;
  margin-bottom: 5px;
  display: block;
}

/* Botão Cadastrar */
.cadastro-form .btn-cadastrar {
  background-color: #4B0082;
  color: white;
  font-weight: bold;
  cursor: pointer;
  border: none;
  transition: background-color 0.3s ease, transform 0.3s ease;
}

.cadastro-form .btn-cadastrar:hover {
  background-color: #DDA0DD; 
}

.cadastro-form .btn-cadastrar:active {
  background-color: #DDA0DD; 
  transform: translateY(2px);
}

/* Separador */
.separator {
  width: 100%;
  height: 1px;
  background-color: yellow;
  margin: 20px 0;
}

/* Estilo para links de registro */
.Login-page {
  text-align: center;
  margin-top: 20px;
  font-size: 14px;
  color: white;
}

.Login-page a {
  color: yellow;
  text-decoration: none;
  font-weight: bold;
}

/* Linha vertical no painel direito */
.right-panel::before {
  content: "";
  position: absolute;
  left: 0;
  top: 0;
  bottom: 0;
  width: 5px;
  background-color:  #cb6ce6;
}

/* Responsividade */
@media (max-width: 768px) {
  .container {
    flex-direction: column;
  }

  .left-panel, .right-panel {
    flex: none;
    width: 100%;
    height: auto;
  }

  .right-panel {
    padding: 20px;
  }
}

</style>