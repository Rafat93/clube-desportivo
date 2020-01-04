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
                label="Selecione"
                item-text="nome"
                item-value="sigla"
                chips
                hint="Selecione uma modalidade."
                persistent-hint
                @change="getInfoModalidade"
              ></v-select>
              <div v-if="modalidadeSelecionada != ''">
                {{modalidadeInfo}}
                <v-select
                  v-model="escalaoSelecionado"
                  :items="modalidadeInfo.graduacoes"
                  label="Selecione"
                  item-text="nome"
                  chips
                  hint="Selecione uma graduação."
                  persistent-hint
                  @change="getInfoModalidade"
                ></v-select>
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
          escalaoSelecionado: '',
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
              .then((modalidades) => {
                this.modalidadeInfo = modalidades;
              });
          }
      },
      created() {
          this.getModalidades();
      }
    }
</script>

<style scoped>

</style>
