<template>
  <v-container fluid class="pa-0" >
    <v-layout row wrap align-end>
      <v-flex xs12 sm12 md12 lg12>
        <v-layout row wrap>
          <v-btn flat small color="error" @click="logout">Sair</v-btn>
          <v-btn flat small color="error" @click="updatePerfil" >Atualizar Perfil</v-btn>         
        </v-layout>
      </v-flex>
    </v-layout>

<br>
<hr>
<br>
    <!--Busca do Repositório-->
    <v-layout align-center justify-center row fill-height>
      <h1 class="name">GitHub</h1>
    </v-layout>
    <br>

    <v-layout align-center justify-center row fill-height>
          <v-flex xs12 sm12 md12 lg12>
            <v-text-field dark v-model="userNameGitHub" placeholder="Digite o nome do usuário" type="text" label="Usuário" single-line solo ></v-text-field>
          <v-layout justify-center>
            <v-btn @click="buscarRepoGitByUser" color="red" dark>Encontrar repositórios</v-btn>
          </v-layout>
          </v-flex>
        </v-layout>
    
    <!--Resultados-->

    <v-layout row align-center justify-center>
      <v-flex xs12 sm12 md12 lg12>
          <v-list two-line style="margin: 20px;">
            <v-list-tile class="item" v-for="repo in reposByUser" :key="repo.length">
              <v-list-tile-content class="item-content">
                <v-list-tile-title class="name">{{ repo.name }} | Avaliação: {{ repo.stargazers_count }}</v-list-tile-title>
                <v-list-tile-sub-title style="color: #fafafa;" >{{ repo.url }}</v-list-tile-sub-title>
              </v-list-tile-content>

              <v-list-tile-action>
                <v-btn icon ripple >
                <router-link to="repo.url"><v-icon color="black" dark>link</v-icon></router-link>
                  
                </v-btn>
              </v-list-tile-action>
            </v-list-tile>            
          </v-list>      
      </v-flex>
  </v-layout>



  </v-container>

</template>


<script>
import firebase, { functions } from "firebase";
import $ from "jquery";

export default {
  name: "home",
  data() {
    return {
      userNameGitHub: "",
      reposByUser: []
    };
  },
  mounted: function() {
    var user = firebase.auth().currentUser;
    if (user != null) {
      // localStorage.name = user.displayName;

      var endereco = firebase
        .database()
        .ref(`users/` + user.uid)
        .on("value", snapshot => {
          localStorage.endereco = JSON.stringify(snapshot.val());
        });
    }
  },
  methods: {
    buscarRepoGitByUser: function() {
      var listaDeRepositorios = [];
      $.getJSON(
        "https://api.github.com" + "/users/" + this.userNameGitHub + "/repos",
        function(repositorisUsuario) {
          if (repositorisUsuario.message == "Not Found") {
            alert("Erro ao consultar repositorio");
            return;
          }

          if (repositorisUsuario != null) {
            repositorisUsuario.forEach(function(repo) {
              var repo = {
                name: repo.name,
                url: repo.url,
                stargazers_count: repo.stargazers_count
              };

              listaDeRepositorios.push(repo);
            });
          }
        }
      ).fail(function() {
        alert("Usuário não encontrado!");
      });
      this.reposByUser = listaDeRepositorios;
    },
    logout: function() {
      firebase
        .auth()
        .signOut()
        .then(() => {
          this.$router.replace("login");
        });
    },
    updatePerfil: function() {
      this.$router.replace("updateProfile");
    }
  }
};


</script>

 <style scoped>
.item{
  margin: 10px;
}

.item-content{
  background-color: #424242;
  border-radius: 10px;
  padding: 10px;
}

.theme--light.v-list{
  background-color: #fafafa
}

.name{
  color: #f44336;
  font-weight: bold;
}

.layout.row{
  flex-direction: row-reverse;
}

</style>