<template>
  <v-data-table
    :headers="headers"
    :items="socios"
    :items-per-page="10"
    class="elevation-1"
  >
  </v-data-table>
</template>

<script>
    export default {
        name: "list",
      data () {
        return {
          headers: [
            {
              text: 'Nº Sócio',
              align: 'left',
              sortable: true,
              value: 'numeroSocio'
            },
            {
              text: 'Nome',
              align: 'left',
              sortable: false,
              value: 'nome'
            },
            {
              text: 'Data Nascimento',
              align: 'left',
              sortable: false,
              value: 'dataNascimento'
            },
            {
              text: 'Nº Identificação Civil',
              align: 'left',
              sortable: true,
              value: 'numIdentificacaoCivil'
            },
            {
              text: 'Nº Identificação Fiscal',
              align: 'left',
              sortable: true,
              value: 'numContribuinte'
            },
            {
              text: 'Morada',
              align: 'left',
              sortable: false,
              value: 'morada'
            },

          ],
          socios: []
        }
      },
      created() {
          this.getSocios();
      },
      methods:{
          getSocios (){
            this.$axios.$get('/api/socios')
              .then((socios) => {
                this.socios = socios;
              }).catch(error => {
                this.$toast.show(error)
                });
            },

      }
    }
</script>

<style scoped>

</style>
