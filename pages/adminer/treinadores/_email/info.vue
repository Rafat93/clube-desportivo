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
            Informações Pessoais
          </v-card-title>
          <v-divider></v-divider>
          <v-card-text>
            <div class="text--primary">
              <p></p>
              <div class="group-form">
                <div class="input">Nome:</div>
                {{treinador.nome}}
              </div>

              <div class="group-form">
                <div class="input">Email:</div>
                {{treinador.email}}
              </div>

              <div class="group-form">
                <div class="input">Nº Cédula:</div>
                {{treinador.numeroCedula}}
              </div>

            </div>
          </v-card-text>
        </v-card>
      </v-col>
      <v-col cols="2">
        <v-card align="center" width="183px">
          <v-card-title class="justify-center">
            Ações
          </v-card-title>
          <v-divider></v-divider>
          <v-card-text class="justify-center">
            <v-btn small color="primary"  width="130px;" style="margin-bottom: 5px;" ><v-icon small>{{'mdi-plus'}}</v-icon> Modalidade</v-btn>
            <v-dialog v-model="dialog" width="700">
              <v-card>
                <v-card-title class="headline grey lighten-2" primary-title>
                  Editar o treinador - {{treinador.nome}}
                </v-card-title>

                <v-card-text>
                  <br>
                  <!--FORMULARIO PARA EDITAR O TREINADOR-->
                  <v-text-field
                    v-model="treinador.nome"
                    :counter="30"
                    :rules="nomeRules"
                    label="Nome"
                    required
                  ></v-text-field>
                  <v-text-field
                    v-model="treinador.numeroCedula"
                    :counter="10"
                    label="Nº Cédula"
                    required
                  ></v-text-field>

                </v-card-text>
                <v-divider></v-divider>

                <v-card-actions>
                  <v-spacer></v-spacer>
                  <v-btn color="primary" text @click="acceptEditar" >
                    Aceitar
                  </v-btn>
                  <v-btn color="error" text @click="cancelEditar">
                    Cancelar
                  </v-btn>
                </v-card-actions>
              </v-card>
              <template v-slot:activator="{ on }">
                <v-btn small v-on="on" color="warning" width="130px;" style="margin-bottom: 5px;" ><v-icon small>{{'mdi-pencil'}}</v-icon> &nbsp; Editar</v-btn>
              </template>
            </v-dialog>

            <!--DIALOG PARA ALTERAR A PASSWORD-->
            <v-dialog v-model="dialog_password" width="700">
              <v-card>
                <v-card-title class="headline grey lighten-2" primary-title>
                  Editar o treinador - {{treinador.nome}} - password
                </v-card-title>

                <v-card-text>
                  <br>
                  <!--FORMULARIO PARA EDITAR A PASSWORD DO TREINADOR-->
                  {{old_password}}
                  <v-text-field
                    v-model="old_password_given"
                    :counter="30"
                    label="Old Password"
                    type="password"
                    required
                  ></v-text-field>
                  <v-text-field
                    v-model="new_password_given"
                    :counter="10"
                    label="Nº Cédula"
                    type="password"
                    required
                  ></v-text-field>
                  <v-text-field
                    v-model="confirmation_password_given"
                    :counter="10"
                    label="Nº Cédula"
                    type="password"
                    required
                  ></v-text-field>

                </v-card-text>
                <v-divider></v-divider>

                <v-card-actions>
                  <v-spacer></v-spacer>
                  <v-btn color="primary" text @click="passwordEditar" >
                    Aceitar
                  </v-btn>
                  <v-btn color="error" text @click="cancelEditar">
                    Cancelar
                  </v-btn>
                </v-card-actions>
              </v-card>
              <template v-slot:activator="{ on }">
                <v-btn small color="warning" v-on="on" width="130px;"  style="margin-bottom: 5px;" ><v-icon small>{{'mdi-pencil'}}</v-icon> &nbsp;Password </v-btn>
              </template>
            </v-dialog>
            <v-btn small color="error" width="130px;"  style="margin-bottom: 5px;" @click="deleteTreinador"><v-icon small>{{'mdi-delete'}}</v-icon> &nbsp;Delete </v-btn>
          </v-card-text>
        </v-card>
      </v-col>
    </v-row>
    <v-row>
      <v-col>
        <v-card>
          <v-card-title class="justify-center">
            Modalidades
          </v-card-title>
          <v-card-text>
            <v-divider></v-divider>
            <div class="text--primary">
                <v-data-table
                  :headers="headers_modalidades"
                  :items="modalidades"
                  :items-per-page="10"
                  class="elevation-1"
                >
                </v-data-table>
            </div>
          </v-card-text>
        </v-card>
      </v-col>
    </v-row>
  </div>

</template>

<script>
    export default {
        name: "info",
      data () {
        return {
          treinador: '',
          old_password: '',
          password_confirmation: '',
          old_password_given: '',
          confirmation_password_given: '',
          new_password_given: '',
          treinadores: [],
          //Snackbar
          color: '',
          mode: '',
          snackbar: false,
          text: '',
          timeout: 4000,
          x: null,
          y: 'top',
          //---------

          // --- Edit dialog ---
          dialog: false,
          nomeRules: [
            v => !!v || 'Name é um campo obrigatório',
            v => (v && v.length <= 30) || 'Nome deve ter até 30 caracteres',
          ],
          // ------------------

          // --- Edit Password dialog ---
          dialog_password: false,
          // ----------

          modalidades:[],
          headers_modalidades:[
            {
              text: 'Nome',
              align: 'left',
              sortable: false,
              value: 'nome'
            },
            {
              text: 'Sigla',
              align: 'left',
              sortable: false,
              value: 'sigla'
            },
          ]



        }
      },
      methods:{
        getTreinador(){
          this.$axios.$get('/api/treinadores/'+this.$route.params.email+'/').then((treinador) => {
            this.treinador = treinador;
            this.old_password = treinador.password;
          });
        },
        getTreinadores(){
          this.$axios.$get('/api/treinadores')
            .then((treinadores) => {
              this.treinadores = treinadores;
            });
        },
        deleteTreinador(){
          let response = confirm('Tem a certeza que pretende eliminar o treinador?');
          if(response == true){
            this.$axios.$delete('/api/treinadores/'+this.treinador.email).then( treinador =>
              {
                this.color = 'green';
                this.text = 'Treinador - '+this.treinador.nome+' - eliminada com sucesso!';
                this.snackbar = true;

                this.getTreinadores();
                setTimeout(() => {
                  this.$router.push('/adminer/treinadores/list');
                }, 2000)
              }
            );
          }
        },
        getModalidades(){
          this.$axios.$get('/api/treinadores/'+this.$route.params.email+'/modalidades/')
            .then((modalidades) => {
              this.modalidades = modalidades;
            });
        },
        cancelEditar(){
          this.dialog = false;
          this.getTreinador();
          this.dialog_password = false;
        },
        acceptEditar(){
          this.$axios.$put('/api/treinadores/'+this.treinador.email+'/',{

            nome: this.treinador.nome,
            password: this.treinador.password,
            email: this.treinador.email,
            numeroCedula: this.treinador.numeroCedula

          }).then(
            () => {
              this.dialog = false;
            }
          ).catch(error => {
            console.log(error)
          })
        },

        passwordEditar(){
          console.log("CHEGOU AQUI");
          if (this.old_password === this.old_password_given){
            console.log("IGUAL");
          }
          /*this.$axios.$put('/api/treinadores/'+this.treinador.email+'/',{

            nome: this.treinador.nome,
            password: this.treinador.password,
            email: this.treinador.email,
            numeroCedula: this.treinador.numeroCedula

          }).then(
            () => {
              this.dialog = false;
            }
          ).catch(error => {
            console.log(error)
          })*/
        },

      },
      created() {
          this.getTreinador();
          this.getModalidades();
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
