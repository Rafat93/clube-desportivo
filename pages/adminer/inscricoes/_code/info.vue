<template>
  <div>
    <v-card>
      <v-card-title class="justify-center">
        Inscricao - {{code}}

      </v-card-title>
      <v-card-text>
        <v-divider></v-divider>
            <div class="text--primary">
              <p></p>
              <div class="group-form">
                <div class="input">Nome:</div>
                {{inscricao.nome}}
              </div>

              <div class="group-form">
                <div class="input">Email:</div>
                {{inscricao.email}}
              </div>

              <div class="group-form">
                <div class="input">Nº de Identificação Fiscal:</div>
                {{inscricao.numContribuinte}}
              </div>

              <div class="group-form">
                <div class="input">Nº de Identificação Civil:</div>
                {{inscricao.numIdentificacaoCivil}}
              </div>

              <div class="group-form">
                <div class="input">Data Nascimento:</div>
                {{inscricao.dataNascimento}}
              </div>
            </div>
      </v-card-text>
    </v-card>


  </div>

</template>

<script>
    export default {
        name: "info",
      data () {
        return {
          inscricao: '',


        }
      },
      computed:{
        code(){
          return this.$route.params.code
        },
      },
      methods:{
          getInscricao(){
            this.$axios.$get('/api/inscricoes/'+this.$route.params.code+'/')
              .then((inscricao) => {
                this.inscricao = inscricao;
              });
          },

      },
      created() {
          this.getInscricao();
      }


    }
</script>

<style scoped>

  .group-form{
    margin-top: 8px;
    margin-bottom: 13px;
    margin-left: 100px;
    font-size: 16px;
  }
  .input{
    font-weight: bold;
    margin-bottom: 3px;
  }
</style>
