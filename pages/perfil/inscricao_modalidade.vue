<template>
    <div>
      <v-row>
        <v-col>
          <v-card>
            <v-card-title class="justify-center">
              <div>Inscrição numa modalidade</div>
            </v-card-title>
            <v-divider></v-divider>
            <v-card-text>
              <v-select
                v-model="modalidadeSelecionada"
                :items="modalidades"
                label="Selecione uma modalidade."
                item-text="nome"
                item-value="sigla"
                chips
                persistent-hint
                @change="getInfoModalidade"
              ></v-select>
              <div v-if="modalidadeSelecionada != ''">
                <v-select
                  v-model="graduacaoSelecionado"
                  :items="graduacoes"
                  label="Selecione uma graduação"
                  item-text="nome"
                  item-value="code"
                  chips
                  persistent-hint
                  @change="getInfoModalidade"
                ></v-select>
              </div>
              <div align="center">
                <v-btn @click="submeter" color="success" :disabled="graduacaoSelecionado == ''">Submeter</v-btn>
                <v-btn to="/perfil/info" color="error" >Cancelar</v-btn>
              </div>


            </v-card-text>

          </v-card>
        </v-col>
      </v-row>

    </div>
</template>

<script>
    export default {
        name: "inscricao_modalidade",
      data () {
        return {
          modalidades:[],
          modalidadeSelecionada:'',
          modalidadeInfo:[],
          graduacaoSelecionado: '',
          graduacoes:[],
        }
      },
      methods:{

          getModalidades(){
            this.$axios.$get('/api/atletas/'+this.$auth.user.sub+'/modalidades_livres/')
              .then((modalidades) => {
                this.modalidades = modalidades;
              });
          },
          getInfoModalidade(){
            this.$axios.$get('/api/modalidades/'+this.modalidadeSelecionada)
              .then((modalidade) => {
                this.modalidadeInfo = modalidade;
              });
            this.$axios.$get('/api/modalidades/'+this.modalidadeSelecionada+'/graduacoes')
              .then((graduacoes) => {
                this.graduacoes = graduacoes;
              });
          },
          submeter(){
            this.$axios.$post('/api/inscricoes/atleta',{
              code: "INSC_"+this.modalidadeSelecionada+"_"+this.$auth.user.sub,
              email: this.$auth.user.sub,
              siglaModalidade: this.modalidadeSelecionada
            }).then(() => {
              this.$router.push('/perfil/info')
            })
              .catch(error => {
                console.log(error)
              })
          },
      },
      created() {
          this.getModalidades();
      }
    }
</script>

<style scoped>

</style>
