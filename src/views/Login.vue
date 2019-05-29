<template>
  
    <v-form v-model="valid" class="login">
      <v-container fluid class="pa-0" grid-list-xl >
        <v-layout  align-center justify-center row >
        <v-flex xs12 sm12 md12 lg12>
            <v-text-field dark v-model="email" :rules="emailRules" type="email" label="E-mail do usuário" single-line solo ></v-text-field>
          </v-flex>
        </v-layout>
        <v-layout>
          <v-flex xs12 sm12 md12 lg12>
            <v-text-field dark v-model="password" :rules="passwordRules" type="password" label="Senha de acesso" required single-line solo ></v-text-field>
          </v-flex>        
        </v-layout>
        <v-layout justify-center>
          <v-btn @click="login" color="red" dark>Entrar</v-btn>
        </v-layout>
        <br>
        <hr>
        <br>
        <v-layout justify-center>
          <p>Novo por aqui? <router-link to="/sign-up">Crie sua conta!</router-link></p>
        </v-layout>
      </v-container>
    </v-form>
  
</template>


<!-- Firebase Auth -->

<script>
  import firebase from 'firebase';
  export default {
    name: 'login',
    data() {
      return {
        email: '',
        password: ''
      }
    },
    methods: {
      login: function() {
        firebase.auth().signInWithEmailAndPassword(this.email, this.password).then(
          (user) => {
            this.$router.replace('home')
          },
          (err) => {
            alert('Oops. ' + err.message)
          }
        );
      }
    }
  }
</script>



<style scoped>  
  .login {
    margin-top: 40%;
    width: 100%;
  }

  p{
    font-size: 16px;
    font-weight: bold;
    text-transform: uppercase;
  }
  
  p a {
    text-decoration: underline;
    cursor: pointer;
    color: #f44336;
  }
</style>