<template>
  <div>
    <v-row>
      <!--COLUNA COM AS TABELAS-->
      <v-col >
        <v-row>
          <v-col>
            <v-card>
              <v-card-title style="background-color: lightsalmon" class="justify-center">
                Nº de Atletas
              </v-card-title>
              <v-card-text style="margin-top: 20px">
                {{atletas.length}}
              </v-card-text>
            </v-card>
          </v-col>
          <v-col>
            <v-card>
              <v-card-title style="background-color: lightsalmon; " class="justify-center">
                Nº de Socios
              </v-card-title>
              <v-card-text style="margin-top: 20px">
                {{atletas.length}}
              </v-card-text>
            </v-card>
          </v-col>
          <v-col>
            <v-card>
              <v-card-title style="background-color: lightsalmon" class="justify-center">
                Nº de Treinadores
              </v-card-title>
              <v-card-text style="margin-top: 20px">
                {{treinadores.length}}
              </v-card-text>
            </v-card>
          </v-col>
        </v-row>
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
                      :headers="headers_atletas"
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
        </v-row>
      </v-col>
      <!--COLUNA COM A INFO-->
      <v-col cols="2">
        <v-card>
          <v-card-title >
            Ações
          </v-card-title>
          <v-divider></v-divider>
          <v-card-text>
            <v-btn small color="primary" @click="adicionarEscalao" width="150px;" style="margin-bottom: 5px;" ><v-icon>{{'mdi-plus'}}</v-icon> Escalão</v-btn>
            <v-btn small color="primary" width="150px;" style="margin-bottom: 5px;" ><v-icon>{{'mdi-plus'}}</v-icon>Treino</v-btn>
            <v-btn small color="primary" @click="adicionarGraduacao" width="150px;"  style="margin-bottom: 5px;" ><v-icon>{{'mdi-plus'}}</v-icon> Graduação </v-btn>
            <v-btn small color="primary" @click="adicionarTreinador" width="150px;" style="margin-bottom: 5px;" ><v-icon>{{'mdi-plus'}}</v-icon>Treinador</v-btn>
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

          atletas: [],
          treinadores:[],
          escaloes:[],
          graduacoes:[],
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

        }
      },
      methods:{
        getAtletas(){
          this.$axios.$get('/api/modalidades/'+this.$route.params.sigla+'/atletas').then((atletas) => {
            this.atletas = atletas;
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
        getEscaloes(){
          this.$axios.$get('/api/modalidades/'+this.$route.params.sigla+'/graduacoes/').then((graduacoes) => {
            this.graduacoes = graduacoes;
          });
        },
        adicionarEscalao(){
          this.$router.push('/adminer/modalidades/'+this.$route.params.sigla+'/criar_escalao');
        },
        adicionarGraduacao(){
          this.$router.push('/adminer/modalidades/'+this.$route.params.sigla+'/criar_graduacao');
        },
        adicionarTreinador(){
          this.$router.push('/adminer/modalidades/'+this.$route.params.sigla+'/adicionar_treinador');
        }


      },
      created() {
          this.getAtletas();
          this.getTreinadores();
          this.getEscaloes();
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
