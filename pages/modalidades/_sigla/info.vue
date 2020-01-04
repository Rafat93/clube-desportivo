<template>
  <div>
    <div v-if="treinadores.length != 0 || escaloes.length != 0 || graduacoes.length != 0 || treinos.length != 0">
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
                      </v-data-table>
                    </div>
                  </div>
                </v-card-text>
              </v-card>
            </v-col>
            <v-col>
              <v-card>
                <v-card-title class="justify-center">
                  Treinos
                  <v-btn small @click="infoTreinos" style="margin-left: 10px;">Info</v-btn>
                </v-card-title>
                <v-card-text>
                  <v-divider></v-divider>
                  <div class="text--primary">
                    <div class="group-form">
                      <v-data-table
                        :headers="headers_treinos"
                        :items="treinos"
                        :items-per-page="10"
                        class="elevation-1"
                      >
                        <template v-slot:item.action="{ item }" >
                          <v-icon @click="deleteTreino(item)">
                            {{'mdi-information-outline'}}
                          </v-icon>
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
                        <template v-slot:item.action="{ item }" >
                          <v-icon @click="deleteEscalao(item)">
                            {{'mdi-information-outline'}}
                          </v-icon>
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
                  Graduações
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
                      ><template v-slot:item.action="{ item }" >
                        <v-icon @click="deleteGraduacao(item)">
                          {{'mdi-information-outline'}}
                        </v-icon>
                      </template>
                      </v-data-table>
                    </div>
                  </div>
                </v-card-text>
              </v-card>
            </v-col>
          </v-row>
        </v-col>
      </v-row>
    </div>
    <div v-if="treinadores.length == 0 || escaloes.length == 0 || graduacoes.length == 0 || treinos.length == 0">
      <v-card>
        <v-card-title class="justify-center">
          Indisponível
        </v-card-title>
        <v-card-text align="center">
          Esta modalidade ainda não se encontra disponivel. Pedimos desculpa pelo incómodo.
        </v-card-text>
      </v-card>
    </div>

  </div>
</template>

<script>
    export default {
        name: "info",
      data () {
        return {
          atletas: [],
          treinadores:[],
          escaloes:[],
          graduacoes:[],
          treinos: [],
          modalidade: '',

          headers_treinos:[
            {
              text: 'Dia Semana',
              align: 'left',
              sortable: false,
              value: 'diaSemana'
            },
            {
              text: 'Hora de Inicio',
              align: 'left',
              sortable: false,
              value: 'horaInicio'
            },
            {
              text: 'Hora de Fim',
              align: 'left',
              sortable: false,
              value: 'horaFim'
            },
            { text: 'Ações', value: 'action', sortable: false },
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
            { text: 'Ações', value: 'action', sortable: false },
          ],
          headers_graduacoes:[
            {
              text: 'Nome',
              align: 'left',
              sortable: false,
              value: 'nome'
            },
            { text: 'Ações', value: 'action', sortable: false },
          ],
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
            { text: 'Ações', value: 'action', sortable: false },
          ],
          headers_treinadores:[
            {
              text: 'Nome',
              align: 'left',
              sortable: false,
              value: 'nome'
            },
            {
              text: 'Email',
              align: 'left',
              sortable: false,
              value: 'email'
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
        getTreinos(){
          this.$axios.$get('/api/modalidades/'+this.$route.params.sigla+'/treinos/').then((treinos) => {
            this.treinos = treinos;
          });
        },
        infoTreinos(){
          this.$router.push('/modalidades/'+this.$route.params.sigla+'/treinos');

        },
      },
      created() {
        this.getModalidade();
        this.getAtletas();
        this.getTreinadores();
        this.getEscaloes();
        this.getAllTreinadores();
        this.getGraduacoes();
        this.getTreinos();
      }
    }
</script>

<style scoped>

</style>
