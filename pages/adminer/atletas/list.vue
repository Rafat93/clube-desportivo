<template>
  <div>
    <v-data-table
      :headers="headers"
      :items="atletas"
      :items-per-page="10"
      class="elevation-1"
    >

    </v-data-table>
  </div>

</template>

<script>
  export default {
    data () {
      return {
        headers: [
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
          }
        ],
        atletas: []
      }
    },
    created () {

      this.$axios.$get('/api/atletas')
        .then((atletas) => {
          this.atletas = atletas
        });


    }
  }
</script>
