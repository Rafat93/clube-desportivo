<template>
  <div>
    <v-row>
      <v-col>
        <v-card>
          <v-card-title class="justify-center">
            {{modalidade.nome}}
          </v-card-title>
        </v-card>
      </v-col>
    </v-row>
    <v-row>
      <!--COLUNA COM AS TABELAS-->
      <v-col >
        <v-row>
          <v-col>
            <v-card>
              <v-card-title class="justify-center">
                Treinadores
              </v-card-title>
              <v-card-text>
                <v-divider></v-divider>
                <div class="text--primary">
                  <div class="group-form">
                    <v-data-table
                      :headers="headers_treinadores"
                      :items="treinadores"
                      :items-per-page="10"
                      class="elevation-1"
                    >
                      <template slot="items" slot-scope="props">
                        <tr @click="props.expanded = !props.expanded">
                          <td class="text-xs">{{ props.item.nome }}</td>
                        </tr>
                      </template>
                    </v-data-table>
                  </div>
                </div>
              </v-card-text>
            </v-card>
          </v-col>
          <v-col>
            <v-card>
              <v-card-title class="justify-center">
                Atletas
              </v-card-title>
              <v-card-text>
                <v-divider></v-divider>
                <div class="text--primary">
                  <div class="group-form">
                    <v-data-table
                      :headers="headers_atletas"
                      :items="atletas"
                      :items-per-page="10"
                      class="elevation-1"
                    >
                      <template slot="items" slot-scope="props">
                        <tr @click="props.expanded = !props.expanded">
                          <td class="text-xs">{{ props.item.nome }}</td>
                          <td class="text-xs">{{ props.item.numeroSocio }}</td>
                        </tr>
                      </template>
                    </v-data-table>
                  </div>
                </div>
              </v-card-text>
            </v-card>
          </v-col>

        </v-row>
        <v-row>
          <v-col>
            <v-card>
              <v-card-title class="justify-center">
                Escalões
              </v-card-title>
              <v-card-text>
                <v-divider></v-divider>
                <div class="text--primary">
                  <div class="group-form">
                    <v-data-table
                      :headers="headers_escaloes"
                      :items="escaloes"
                      :items-per-page="10"
                      class="elevation-1"
                    >
                      <template slot="items" slot-scope="props">
                        <tr @click="props.expanded = !props.expanded">
                          <td class="text-xs">{{ props.item.nome }}</td>
                        </tr>
                      </template>
                    </v-data-table>
                  </div>
                </div>
              </v-card-text>
            </v-card>
          </v-col>
          <v-col>
            <v-card>
              <v-card-title class="justify-center">
                Graduações {{graduacoes}}
              </v-card-title>
              <v-card-text>
                <v-divider></v-divider>
                <div class="text--primary">
                  <div class="group-form">
                    <v-data-table
                      :headers="headers_graduacoes"
                      :items="graduacoes"
                      :items-per-page="10"
                      class="elevation-1"
                    >
                    </v-data-table>
                  </div>
                </div>
              </v-card-text>
            </v-card>
          </v-col>
        </v-row>
      </v-col>
      <!--COLUNA COM AS ACOES-->
      <v-col cols="2">
        <v-row>
          <v-card style="margin-top: 12px;" align="center" width="183px">
            <v-card-title class="justify-center">
              Ações
            </v-card-title>
            <v-divider></v-divider>
            <v-card-text class="justify-center">
              <v-btn small color="primary" @click="adicionarEscalao" width="130px;" style="margin-bottom: 5px;" ><v-icon>{{'mdi-plus'}}</v-icon> Escalão</v-btn>
              <v-btn small color="primary" width="130px;" style="margin-bottom: 5px;" ><v-icon>{{'mdi-plus'}}</v-icon>Treino</v-btn>

              <!--POPUP PARA ADICIONAR UMA GRADUACAO A ESTA MODALIDADE-->
              <v-dialog v-model="dialog_graduacao" width="500">
                <v-card>
                  <v-card-title class="headline grey lighten-2" primary-title>
                    Adicionar Graduação a {{modalidade.nome}}
                  </v-card-title>
                  <v-card-text>
                    <v-form ref="form" v-model="valid" >
                      <p class="subtitle-1 text-center">Criação de Graduação</p>
                      <v-text-field
                        v-model="code"
                        :counter="10"
                        :rules="codeRules"
                        label="Código"
                        required
                      ></v-text-field>
                      <v-text-field
                        v-model="nome"
                        :counter="20"
                        :rules="nomeRules"
                        label="Nome"
                        required
                      ></v-text-field>
                    </v-form>
                  </v-card-text>
                  <v-divider></v-divider>

                  <v-card-actions>
                    <v-spacer></v-spacer>
                    <v-btn color="primary" :disabled="!valid" text @click="adicionarGraduacao">
                      Aceitar
                    </v-btn>
                    <v-btn color="error" text @click="cancelGraduacao">
                      Cancelar
                    </v-btn>
                  </v-card-actions>
                </v-card>
                <template v-slot:activator="{ on }">
                  <v-btn small color="primary"width="130px;" v-on="on" style="margin-bottom: 5px;" ><v-icon>{{'mdi-plus'}}</v-icon> Graduação </v-btn>
                </template>
              </v-dialog>


              <!--POPUP PARA ADICIONAR UM TREINADOR A ESTA MODALIDADE-->
              <v-dialog v-model="dialog_treinador" width="500">
                <v-card>
                  <v-card-title class="headline grey lighten-2" primary-title>
                    Adicionar Treinador a {{modalidade.nome}}
                  </v-card-title>

                  <v-card-text>
                    Por favor, selecione um treinador para adicionar à modalidade de {{modalidade.nome}}.

                    <v-select
                      v-model="treinadorSelecionado"
                      :items="all_treinadores"
                      label="Selecione"
                      item-text="nome"
                      item-value="email"
                      chips
                      hint="Selecione um treinador."
                      persistent-hint
                    ></v-select>
                  </v-card-text>
                  <v-divider></v-divider>

                  <v-card-actions>
                    <v-spacer></v-spacer>
                    <v-btn color="primary" text @click="adicionarTreinador">
                      Aceitar
                    </v-btn>
                    <v-btn color="error" text @click="dialog_treinador = false">
                      Cancelar
                    </v-btn>
                  </v-card-actions>
                </v-card>
              <template v-slot:activator="{ on }">
               <v-btn small color="primary" v-on="on" width="130px;" style="margin-bottom: 5px;" ><v-icon>{{'mdi-plus'}}</v-icon>Treinador</v-btn>
              </template>
            </v-dialog>


              <v-btn small color="warning" @click="" width="130px;" style="margin-bottom: 5px;" ><v-icon small>{{'mdi-pencil'}}</v-icon>Editar</v-btn>
            </v-card-text>
          </v-card>
        </v-row>

        <!--COLUNA COM A INFO-->
        <v-row>
          <v-card style="margin-top: 5px;" align="center" width="183px">
            <v-card-title class="justify-center">
              Informações
            </v-card-title>
            <v-divider></v-divider>
            <v-card-text>
              <p class="font-weight-medium">Nº Atletas: {{atletas.length}}</p>
              <p class="font-weight-medium">Nº Treinadores: {{treinadores.length}}</p>
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

          dialog_treinador: false,
          dialog_graduacao: false,
          treinadorSelecionado: '',
          modalidade: '',
          atletas: [],
          treinadores:[],
          treinadoresExternos:[],
          all_treinadores:[],
          escaloes:[],
          graduacoes:[],

          //---- CRIAÇÃO DE GRADUAÇÃO -----

          code: '',
          codeRules:[
            v => !!v || 'Código é um campo obrigatório',
            v => (v && v.length <= 10) || 'Código deve ter até 10 caracteres',
          ],

          nome: '',
          nomeRules:[
            v => !!v || 'Nome é um campo obrigatório',
            v => (v && v.length <= 20) || 'Nome deve ter até 20 caracteres',
          ],
          // -----------------------
          headers_atletas:[
            {
              text: 'Nome',
              align: 'left',
              sortable: false,
              value: 'nome'
            },
            {
              text: 'Nº Sócio',
              align: 'left',
              sortable: false,
              value: 'numeroSocio'
            },
          ],
          headers_escaloes:[
            {
              text: 'Nome',
              align: 'left',
              sortable: false,
              value: 'nome'
            },
            {
              text: 'Idade Min',
              align: 'left',
              sortable: false,
              value: 'idadeMin'
            },
            {
              text: 'Idade Max',
              align: 'left',
              sortable: false,
              value: 'idadeMax'
            },
          ],
          headers_graduacoes:[
            {
              text: 'Nome',
              align: 'left',
              sortable: false,
              value: 'nome'
            },
          ],
          headers_treinadores:[
            {
              text: 'Nome',
              align: 'left',
              sortable: false,
              value: 'nome'
            },
            {
              text: 'Nº Cédula',
              align: 'left',
              sortable: false,
              value: 'numeroCedula'
            },
          ],


        }
      },
      methods:{
        getModalidade(){
          this.$axios.$get('/api/modalidades/'+this.$route.params.sigla).then((modalidade) => {
            this.modalidade = modalidade;
          });
        },
        getAtletas(){
          this.$axios.$get('/api/modalidades/'+this.$route.params.sigla+'/atletas').then((atletas) => {
            this.atletas = atletas;
          });
        },
        getAllTreinadores(){
          this.$axios.$get('/api/modalidades/'+this.$route.params.sigla+'/treinadores_livres').then((treinadores) => {
            this.all_treinadores = treinadores;
          });
        },
        getTreinadores(){
          this.$axios.$get('/api/modalidades/'+this.$route.params.sigla+'/treinadores').then((treinadores) => {
            this.treinadores = treinadores;
          });
        },
        getEscaloes(){
          this.$axios.$get('/api/modalidades/'+this.$route.params.sigla+'/escaloes/').then((escaloes) => {
            this.escaloes = escaloes;
          });
        },
        getGraduacoes(){
          this.$axios.$get('/api/modalidades/'+this.$route.params.sigla+'/graduacoes/').then((graduacoes) => {
            this.graduacoes = graduacoes;
          });
        },
        adicionarEscalao(){
          this.$router.push('/adminer/modalidades/'+this.$route.params.sigla+'/criar_escalao');
        },
        adicionarGraduacao(){
          if (this.$refs.form.validate()) {

            this.$axios.$post('/api/graduacoes/', {
              code: this.code,
              nome: this.nome,
              siglaModalidade: this.$route.params.sigla

            })
              .then(() => {
               this.dialog_graduacao = false;
               this.getGraduacoes;
              })
              .catch(error => {
                console.log(error)
              })
          }
        },
        adicionarTreinador(){
          this.$axios.$put('/api/treinadores/'+this.treinadorSelecionado+'/modalidade/enroll/'+this.$route.params.sigla,{
            email: this.treinadorSelecionado,
            sigla: this.$route.params.sigla,
          }).
          then(()=>{
              this.getTreinadores();
              this.dialog_treinador = false;
              this.treinadorSelecionado = '';
              this.getAllTreinadores();
          }
          ).catch(error => {
            console.log(error);
          });
        },
        treinadoresFilter(){
          for (var i = 0; i <= this.all_treinadores.length; i++){
            for (var j = 0; j <= this.treinadores.length; j++){
              console.log("entrou");
              console.log(this.all_treinadores[i])
            }
          }
        },
        cancelGraduacao(){
          this.dialog_graduacao= false;
          this.code = '';
          this.nome = '';
          this.$refs.form.reset();
          this.$refs.form.resetValidation();
        }
      },
      created() {
          this.getModalidade();
          this.getAtletas();
          this.getTreinadores();
          this.getEscaloes();
          this.getAllTreinadores();
          this.treinadoresFilter();
          this.getGraduacoes();
      }
    }
</script>

<style scoped>
  .group-form{
    margin-top: 8px;
    margin-bottom: 13px;
    font-size: 16px;
  }
  .input{
    font-weight: bold;
    margin-bottom: 3px;
  }
</style>
