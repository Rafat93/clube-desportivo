<template>
  <div>
    <v-row>
      <v-col>
        <v-card>
          <v-card-title class="justify-center">
            <div>Informações Pessoais</div>
            <v-btn small style="margin-left: 20px;" @click="editarPerfil">Editar</v-btn>
          </v-card-title>
          <v-divider></v-divider>
          <v-card-text>
            <div class="text--primary">
              <p></p>
              <v-row>
                <v-col>
                  <div class="group-form" v-if="this.$auth.user.groups == 'Atleta' || this.$auth.user.groups == 'Socio'">
                    <div class="input" >Nº Socio:</div>
                    {{user.numeroSocio}}
                  </div>
                  <div class="group-form" v-if="this.$auth.user.groups == 'Treinador'">
                    <div class="input" >Nº Cédula:</div>
                    {{user.numeroCedula}}
                  </div>
                  <div class="group-form">
                    <div class="input">Nome:</div>
                    {{user.nome}}
                  </div>
                  <div class="group-form">
                    <div class="input">Email:</div>
                    {{user.email}}
                  </div>
                  <div class="group-form" v-if="this.$auth.user.groups == 'Atleta' || this.$auth.user.groups == 'Socio'">
                    <div class="input">Data de Nascimento:</div>
                    {{user.dataNascimento}}
                  </div>
                </v-col>
                <v-col>
                  <div v-if="this.$auth.user.groups == 'Atleta' || this.$auth.user.groups == 'Socio'">
                    <div class="group-form">
                      <div class="input">Nº Identificação Civil:</div>
                      {{user.numIdentificacaoCivil}}
                    </div>
                    <div class="group-form">
                      <div class="input">Nº Identificação Fiscal:</div>
                      {{user.numContribuinte}}
                    </div>
                    <div class="group-form">
                      <div class="input">Morada:</div>
                      {{user.morada}}
                    </div>
                  </div>
                </v-col>
                <v-col>
                  <v-btn small style="margin-bottom: -50px;" @click="redirectCriar"><v-icon>{{'mdi-plus'}}</v-icon> Inscrição numa Modalidade</v-btn>
                </v-col>
              </v-row>
            </div>
          </v-card-text>
        </v-card>
      </v-col>
    </v-row>
    <v-row v-if="this.$auth.user.groups == 'Atleta' || this.$auth.user.groups == 'Treinador'">
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
              ><template v-slot:item.action="{ item }" >
                <v-icon
                  @click="redirectInfo(item)"
                >
                  {{'mdi-information-outline'}}
                </v-icon>
              </template>
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
      data: () => ({
        user: '',
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
          { text: 'Actions', value: 'action', sortable: false },
        ]
      }),
      methods:{
          getUser(){
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
          },
        getModalidades(){
          if(this.$auth.user.groups == 'Treinador' ){
            this.$axios.$get('/api/treinadores/'+this.$auth.user.sub+'/modalidades').then((modalidades) => {
              this.modalidades = modalidades;
            });
          }
          if(this.$auth.user.groups == 'Atleta' ) {
            this.$axios.$get('/api/atletas/'+this.$auth.user.sub+'/modalidades').then((modalidades) => {
              this.modalidades = modalidades;
            });
          }

        },
        editarPerfil(){
          this.$router.push("/perfil/"+this.user.email+"/editar");
        },
        redirectInfo(item){
          this.$router.push("/modalidades/"+item.sigla+"/info");
        },
        redirectCriar(){
          this.$router.push("/perfil/inscricao_modalidade");
        }

      },
      created() {
          this.getUser();
          this.getModalidades();
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
