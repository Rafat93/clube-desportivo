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
  import { mdiMagnify } from '@mdi/js';
    export default {
        name: "list",
      data () {
        return {
          icon: mdiMagnify,
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
            { text: 'Actions', value: 'action', sortable: false },

          ],
          treinadores: [],
        }
      },
      methods:{
        redirectInfo(item){
          this.$router.push('/adminer/treinadores/'+item.email+'/info');
        },

      },
      created () {

        this.$axios.$get('/api/treinadores')
          .then((treinadores) => {
            this.treinadores = treinadores;
          });


      }
    }
</script>

<style scoped>

</style>
