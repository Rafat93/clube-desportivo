<template>
  <div>
    <v-card>
      <v-card-title class="justify-center">
        {{this.$auth.user.sub}}{{this.$auth.user.groups}}
        {{this.$auth.user}}
        {{user}}
      </v-card-title>
      <v-card-text>
        <v-divider></v-divider>
        <div class="text--primary">
          <p></p>
          <div class="group-form">
            <div class="input">Nome:</div>

          </div>

          <div class="group-form">
            <div class="input">Email:</div>
            {{this.$auth.user.email}}
          </div>

          <div class="group-form">
            <div class="input">Nº de Identificação Fiscal:</div>
          </div>

          <div class="group-form">
            <div class="input">Nº de Identificação Civil:</div>
          </div>

          <div class="group-form">
            <div class="input">Data Nascimento:</div>
          </div>

        </div>
      </v-card-text>
    </v-card>
  </div>

</template>

<script>
    export default {
        name: "info",
      data: () => ({
        user: '',
      }),
      methods:{
          getUser(){
            console.log(this.$auth.user.sub);
            if(this.$auth.user.groups == 'Treinador'){
              this.$axios.$get('/api/treinadores/'+this.$auth.user.sub).then((user) => {
                this.user = user;
              });
            }
            if(this.$auth.user.groups == 'Atleta'){
              this.$axios.$get('/api/atletas/'+this.$auth.user.sub).then((user) => {
                this.user = user;
              });
            }
            if(this.$auth.user.groups == 'Socio'){
              this.$axios.$get('/api/socios/'+this.$auth.user.sub).then((user) => {
                this.user = user;
              });
            }
          }
      },
      created() {
          this.getUser();
      }

    }
</script>

<style scoped>

</style>
