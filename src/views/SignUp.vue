<template>

   <v-form v-model="valid" class="signup">
      <v-container fluid class="pa-1" grid-list-xl>
      <v-layout  align-center justify-center row > 
        <h2>CADASTRO</h2>
      </v-layout>  
        <v-layout  align-center justify-center row > 
        <v-flex xs12 sm12 md12 lg12>
            <h3>Usuário</h3>
            <v-text-field dark v-model="email" :rules="emailRules" type="email" label="Digite seu e-mail..." single-line solo ></v-text-field>
            <h3>Senha</h3>
            <v-text-field dark v-model="password" :rules="passwordRules" type="password" label="Escolha uma senha..." required single-line solo ></v-text-field>
          </v-flex>
        </v-layout>
         <v-layout align-center justify-center row fill-height>
          <v-flex xs12 sm12 md12 lg12>
            <v-layout  align-center justify-center row > 
              <h2>Preencha seu endereço</h2>
            </v-layout>
           </v-flex>        
        </v-layout>
        <v-layout align-center justify-center row fill-height>
          <v-flex xs12 sm12 md12 lg12>
            <v-text-field dark type="text" maxlength="8" pattern="([0-9]{8})" v-model="cep" label="Digite o CEP de sua residência..." required single-line solo ></v-text-field>
          <v-layout justify-center>
            <v-btn @click="buscar" color="red" dark>Encontrar meu endereço</v-btn>
          </v-layout>
          </v-flex>
        </v-layout>
        <v-layout>
          <v-flex xs12 sm12 md12 lg12>
            <h3>Endereço</h3>
            <v-text-field dark type="text" v-model="endereco.logradouro" single-line solo ></v-text-field>
            <h3>Número</h3>
            <v-text-field dark type="number" v-model="endereco.numero" single-line solo ></v-text-field>
            <h3>Complemento</h3>
            <v-text-field dark type="text" v-model="endereco.complemento" single-line solo ></v-text-field>
            <h3>Bairro</h3>
            <v-text-field dark type="text" v-model="endereco.bairro" solo ></v-text-field>
            <h3>Cidade</h3>
            <v-text-field dark type="text" v-model="endereco.localidade" single-line solo ></v-text-field>
            <h3>Estado</h3>
            <v-text-field dark type="text" v-model="endereco.uf" single-line solo ></v-text-field>
          </v-flex>        
        </v-layout>
        <v-layout align-center justify-center row fill-height>
          <v-btn @click="signUp" color="red" dark>Cadastrar Usuário</v-btn>
        </v-layout>
        <br>
        <hr>
        <br>
        <v-layout align-center justify-center row fill-height>
          <p><router-link to="/login">Voltar para Login</router-link></p>
        </v-layout>
      </v-container>
    </v-form>

  
</template>

 <script>
import firebase from "firebase";
import $ from "jquery";
import { constants } from "crypto";
export default {
  name: "signUp",
  data: () => {
    return {
      email: "",
      password: "",
      cep: "",
      endereco: {
        cep: "",
        logradouro: "",
        numero: "",
        complemento: "",
        bairro: "",
        localidade: "",
        uf: ""
      },
      naoLocalizado: false
    };
  },
  methods: {
    signUp: function() {

      // validate
      if (
        this.email &&
        this.password &&
        this.endereco.cep &&
        this.endereco.logradouro &&
        this.endereco.numero &&
        this.endereco.bairro &&
        this.endereco.localidade &&
        this.endereco.uf
      ) {
        firebase
          .auth()
          .createUserWithEmailAndPassword(this.email, this.password)
          .then(
            user => {
              console.log(user);
              var userLogado = firebase.auth().currentUser;
              console.log("UID usuário logado: " + userLogado.uid);
              console.log("Endereço atualizado: ", this.endereco);
              firebase
                .database()
                .ref("users/" + userLogado.uid)
                .set(this.endereco);
              console.log(this.endereco.logradouro);
              console.log("Usuário criado e autenticado");
              this.$router.replace("home");
            },
            err => {
              alert("Oops. " + err.message);
            }
          );
      } else {
        alert("Por favor preencha todos os campos ");
      }
    },
    buscar: function() {
      if (this.cep.length == 8) {
        this.naoLocalizado = false;

        if (/^[0-9]{8}$/.test(this.cep)) {
          $.getJSON("https://viacep.com.br/ws/" + this.cep + "/json/", function(
            enderecoRetornado
          ) {
            if (enderecoRetornado.erro) {
              this.endereco = {};
              this.naoLocalizado = true;
              return;
            }
            console.log("Atualizando endereço...");
            console.log("Endereço retornado: ", enderecoRetornado);
            this.endereco = {
              cep: enderecoRetornado.cep,
              logradouro: enderecoRetornado.logradouro,
              numero: "",
              complemento: enderecoRetornado.complemento,
              bairro: enderecoRetornado.bairro,
              localidade: enderecoRetornado.localidade,
              uf: enderecoRetornado.uf
            };
            console.log("Endereço interno atualizado: ", this.endereco);
            localStorage.endereco = JSON.stringify(this.endereco);
          });
          if (localStorage.endereco) {
            this.endereco = JSON.parse(localStorage.endereco);
          }
        } else {
          alert(
            "Por favor preencher o campo CEP corretamente com 8 caracteres."
          );
        }
      } else {
        alert("Por favor preencher o campo CEP corretamente com 8 caracteres.");
      }
    }
  }
};
</script>

 <style scoped>
.signup {
  width: 100%;
}

h2 {
  text-transform: uppercase;
}

p {
  text-transform: uppercase;
  font-weight: bold;
}

p a {
  text-decoration: none;
  font-size: 17px;
  color: #f44336;
}
</style>