<template>
  <div>

    <v-data-table
      :headers="headers"
      :items="modalidades"
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
              text: 'Sigla',
              align: 'left',
              sortable: false,
              value: 'sigla'
            },
            {
              text: 'Nome',
              align: 'left',
              sortable: false,
              value: 'nome'
            },
            {
              text: 'Época Desportiva',
              align: 'left',
              sortable: false,
              value: 'epocaDesportiva'
            },
            { text: 'Actions', value: 'action', sortable: false },

          ],
          modalidades: []
        }
      },
      methods:{
        redirectInfo(item){
          this.$router.push('/modalidades/'+item.sigla+'/info');
        },
      },
      created () {
        this.$axios.$get('/api/modalidades')
          .then((modalidades) => {
            this.modalidades = modalidades;
          });
      }
    }
</script>

<style scoped>

</style>
