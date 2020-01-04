<template>
  <div>
    <v-row>
      <v-col>
        <v-card>
          <v-card-title class="justify-center">
            <div>Sobre o {{this.clube.nome}} </div>
            <div v-if="this.$auth.user.groups == 'Administrador'">
              <v-dialog v-model="dialog" width="500">
                <v-card>
                  <v-card-title class="headline grey lighten-2" primary-title>
                    Editar informação do clube
                  </v-card-title>
                  <v-card-text>
                    <v-text-field
                      v-model="clube.nome"
                      :counter="30"
                      :rules="nomeRules"
                      label="Nome"
                      required
                    ></v-text-field>
                    <v-text-field
                      v-model="clube.sigla"
                      :counter="8"
                      :rules="siglaRules"
                      label="Sigla"
                      required
                    ></v-text-field>
                    <v-text-field
                      v-model="clube.email"
                      :counter="20"
                      :rules="emailRules"
                      label="Email"
                      required
                    ></v-text-field>
                    <v-text-field
                      v-model="clube.telefone"
                      :counter="9"
                      :rules="telefoneRules"
                      label="Telefone"
                      type="number"
                      required
                    ></v-text-field>
                    <v-text-field
                      v-model="clube.descricao"
                      :counter="500"
                      label="Descrição"
                      required
                    ></v-text-field>

                  </v-card-text>
                  <v-divider></v-divider>

                  <v-card-actions>
                    <v-spacer></v-spacer>
                    <v-btn color="primary" text @click="updateClube">
                      Aceitar
                    </v-btn>
                    <v-btn color="error" text @click="dialog = false">
                      Cancelar
                    </v-btn>
                  </v-card-actions>
                </v-card>
                <template v-slot:activator="{ on }">
                  <v-btn v-on="on" style="margin-left: 10px;"> Editar</v-btn>
                </template>
              </v-dialog>
            </div>
          </v-card-title>
          <v-divider></v-divider>
          <v-card-text class="text--primary">
            <div class="group-form">

              <div class="input">Nome:</div> {{this.clube.nome}} - {{this.clube.sigla}}
            </div>
            <div class="group-form">
              <div class="input">Morada:</div> {{this.clube.morada}}
            </div>
            <div class="group-form">
              <div class="input" >Contactos:</div>
              <p><v-icon>{{'mdi-phone'}}</v-icon>: +351 {{this.clube.telefone}}</p>
              <p><v-icon>{{'mdi-email'}}</v-icon>: {{this.clube.email}}</p>
            </div>
            <div class="group-form" v-if="this.clube.descricao != ''">
              <div class="input">Descrição:</div>
              <p>{{this.clube.descricao}}</p>
            </div>
          </v-card-text>
        </v-card>
      </v-col>
    </v-row>
  </div>
</template>

<script>
    export default {
        name: "sobre",
      data: () => ({
        clube: '',
        dialog: false,
        nomeRules:[
          v => !!v || 'Nome é um campo obrigatório',
          v => (v && v.length <= 30) || 'Nome deve ter até 30 caracteres',
        ],
        siglaRules:[
          v => !!v || 'Sigla é um campo obrigatório',
          v => (v && v.length <= 8) || 'Sigla deve ter até 8 caracteres',
        ],
        telefoneRules:[
          v => !!v || 'Telefone é um campo obrigatório',
          v => (v && v.length ===9 ) || 'Telefone deve ter 9 caracteres.',
        ],
        emailRules: [
          v => !!v || 'Email é um campo obrigatório',
          v => /.+@.+\..+/.test(v) || 'E-mail deve ser válido',
        ],
      }),
      methods:{
          getClube(){
            this.$axios.$get('/api/clube/').then((clube) => {
              this.clube = clube;
            });
          },
          updateClube(){
            this.$axios.$put('/api/clube/'+this.clube.nif, {
              sigla: this.clube.sigla,
              nome: this.clube.nome,
              email: this.clube.email,
              descricao: this.clube.descricao,
              morada: this.clube.morada,
              telefone: this.clube.telefone
            })
              .then(() => {
              this.dialog = false;
            }).catch(error => {
              console.log(error)
            });
          },

      },
      created() {
          this.getClube();
      }

    }
</script>

<style scoped>
  .group-form{
    margin-top: 8px;
    margin-bottom: 13px;
    margin-left: 50px;
    font-size: 16px;
  }
  .input{
    font-weight: bold;
    margin-bottom: 3px;
  }
</style>
