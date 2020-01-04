<template>
  <div>
    <v-row>
      <v-col cols="4">
        <div style="display: block">
          <v-text-field
          v-model="search"
          label="Pesquisa"
          hide-details
        ></v-text-field>
        </div>
      </v-col>
      <v-col>
        <v-col align="center">
          <v-btn small  @click="redirectCriar"><v-icon>{{'mdi-plus'}}</v-icon>Treinador</v-btn>
        </v-col>
      </v-col>
    </v-row>

    <v-data-table
      :headers="headers"
      :items="treinadores"
      :items-per-page="10"
      class="elevation-1"
      :search="search"
    >
      <template v-slot:item.action="{ item }" >
        <v-icon
          @click="redirectInfo(item)"
        >
          {{'mdi-information-outline'}}
        </v-icon>
      </template>
    </v-data-table>

  </div>

</template>

<script>
    export default {
        name: "list",
      data () {
        return {

          dialog: false,
          search: '',
          headers: [
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
            {
              text: 'Email',
              align: 'left',
              sortable: false,
              value: 'email'
            },
            { text: 'Ações', value: 'action', sortable: false },

          ],
          treinadores: [],
        }
      },
      methods:{
        redirectInfo(item){
          this.$router.push('/adminer/treinadores/'+item.email+'/info');
        },
        redirectCriar(){
          this.$router.push('/adminer/treinadores/criar_treinador');
        },
        getTreinadores(){
          this.$axios.$get('/api/treinadores')
            .then((treinadores) => {
              this.treinadores = treinadores;
            });
        },
        redirectCriar(){
          this.$router.push('/adminer/socios/criar_socio');
        },

      },
      created () {
        this.getTreinadores();
      }
    }
</script>

<style scoped>

</style>
