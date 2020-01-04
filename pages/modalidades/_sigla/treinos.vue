<template>
  <div>
    <v-card style="margin-bottom: 10px;">
      <v-card-title class="justify-center">
        Informação sobre os treinos de {{modalidade.nome}}
      </v-card-title>
    </v-card>
    <v-data-table
      :headers="headers"
      :items="treinos"
      :items-per-page="10"
      class="elevation-1"
    >
    </v-data-table>
  </div>

</template>

<script>
    export default {
        name: "treinos",
        data () {
          return{
            treinos:[],
            modalidade: '',
            headers:[
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
              {
                text: 'Escalão',
                align: 'left',
                sortable: false,
                value: 'codeEscalao'
              },{
                text: 'Grau',
                align: 'left',
                sortable: false,
                value: 'codeGraduacao'
              },
              {
                text: 'Treinador',
                align: 'left',
                sortable: false,
                value: 'emailTreinador'
              },

            ],
          }

        },
      methods:{
          getTreinos(){
            this.$axios.$get('/api/modalidades/'+this.$route.params.sigla+'/treinos/').then((treinos) => {
              this.treinos = treinos;
            });
          },
          getModalidade(){
            this.$axios.$get('/api/modalidades/'+this.$route.params.sigla).then((modalidade) => {
              this.modalidade = modalidade;
            });
          }
      },
        created() {
          this.getTreinos();
          this.getModalidade();
        }
    }
</script>

<style scoped>

</style>
