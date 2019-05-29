<template>
   <v-form v-model="valid">
      <v-container fluid class="pa-1" grid-list-xl>
       <v-btn @click="backToHome" color="red" flat small dark>Voltar</v-btn>
        <v-layout  align-center justify-center row > 
          <h2>Atualização de cadastro</h2>
        </v-layout>  
          <v-layout  align-center justify-center row > 
            <v-flex xs12 sm12 md12 lg12>
              <v-text-field dark type="text" v-model="displayName" placeholder="Nome Completo" single-line solo ></v-text-field>
            </v-flex>
          </v-layout>
          <v-layout align-center justify-center row>
            <v-btn @click="updateProfileUser" color="red" dark>Atualizar</v-btn>
          </v-layout>
      </v-container>
   </v-form>


  
</template>

 <script>
import firebase from "firebase";
import $ from "jquery";
export default {
  name: "updateProfile",
  data() {
    return {
      displayName: ""
    };
  },
  methods: {
    updateProfileUser: function() {
      var userLogado = firebase.auth().currentUser;
      localStorage.name = this.displayName;
      userLogado
        .updateProfile({
          displayName: this.displayName
        })
        .then(function() {
          console.log("Nome do usuário atualizado com sucesso!");
        });
      this.$router.replace("home");
    },
    backToHome: function() {
      this.$router.replace("home");
    }
  }
};
</script>

 <style scoped>
.sign-up {
  margin-top: 40px;
}
input {
  margin: 10px 0;
  width: 20%;
  padding: 15px;
}
span {
  display: block;
  margin-top: 20px;
  font-size: 11px;
}
.text-danger {
  position: absolute;
  top: 20px;
  font-size: 12px;
}
</style>