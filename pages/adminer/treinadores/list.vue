<template>
  <div>
    <v-data-table
      :headers="headers"
      :items="treinadores"
      :items-per-page="10"
      class="elevation-1"
    >
      <template v-slot:item.action="{ item }">

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
          headers: [
            {
              text: 'Nome',
              align: 'left',
              sortable: false,
              value: 'nome'
            },
            { text: 'Actions', value: 'action', sortable: false },

          ],
          treinadores: []
        }
      },
      methods:{
        redirectInfo(item){
          this.$router.push('/adminer/treinadores/'+item.sigla+'/info');
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
