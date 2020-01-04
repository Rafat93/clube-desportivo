<template>
  <div>
    <v-snackbar
      v-model="snackbar"
      :bottom="y === 'bottom'"
      :color="color"
      :left="x === 'left'"
      :multi-line="mode === 'multi-line'"

      :timeout="timeout"
      :top="y === 'top'"
      :vertical="mode === 'vertical'"
    >
      {{ text }}
      <v-btn dark text @click="snackbar = false">
        Close
      </v-btn>
    </v-snackbar>

    <v-row>
      <v-col>
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
      </v-col>
      <v-col cols="2">
        <v-row>
          <v-card style="margin-top: 12px;" align="center" width="183px">
            <v-card-title class="justify-center">
              Ações
            </v-card-title>
            <v-divider></v-divider>
            <v-card-text class="justify-center">
              <v-btn small color="warning" @click="aceitarInscricao" width="130px;" style="margin-bottom: 5px;" ><v-icon small>{{'mdi-check-box-outline'}}</v-icon>Aceitar</v-btn>
              <v-btn small color="error" @click="deleteInscricao" width="130px;" style="margin-bottom: 5px;" ><v-icon small>{{'mdi-delete'}}</v-icon>Remover</v-btn>
            </v-card-text>
          </v-card>
        </v-row>
      </v-col>
    </v-row>
  </div>

</template>

<script>
    export default {
        name: "info",
      data () {
        return {
          inscricao: '',

          // ---- SNACKBAR INFO -----
          color: '',
          mode: '',
          snackbar: false,
          text: '',
          timeout: 4000,
          x: null,
          y: 'top',
          // ------------------------
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
        aceitarInscricao() {
          this.$axios.$put('api/inscricoes/' + this.$route.params.code + '/confirm')
            .then(() => {
              // show snackbar

              this.text = 'Aceite com sucesso!';
              this.color = 'green';
              this.snackbar = true;


              setTimeout(() => {
                this.$router.push('/adminer/inscricoes/list');
              }, 2000);
            })
            .catch(error => {
              this.color = 'red';
              this.text = 'ERRO: Sócio com o email ' + item.email + ' já existe.';
              this.snackbar = true;
          });
        },
        deleteInscricao() {
          let response = confirm('Tem aa certeza que deseja remover a inscrição?');
          if (response == true){
            this.$axios.$delete('/api/inscricoes/'+this.$route.params.code)
              .then(() => {
                this.color = 'green';
                this.text = 'Inscrição com o código - '+this.$route.params.code+' - eliminada com sucesso!';
                this.snackbar = true;

                setTimeout(() => {
                  this.$router.push('/adminer/inscricoes/list');
                }, 2000);
              }
            ).catch(error => {
              console.log(error)
            });
          }
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
