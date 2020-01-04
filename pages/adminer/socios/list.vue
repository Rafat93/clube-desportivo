<template>

  <v-data-table
    :headers="headers"
    :items="treinos"
    :items-per-page="10"
    class="elevation-1"
  >
    <template v-slot:item.action="{ item }" >
      <v-icon
        @click="redirectInfo(item)"
      >
        {{'mdi-information-outline'}}
      </v-icon>
    </template>
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
              text: 'Email',
              align: 'left',
              sortable: false,
              value: 'email'
            },
            { text: 'Actions', value: 'action', sortable: false },

          ],
          socios: []
        }
      },
      created() {
          this.getSocios();
      },
      methods:{
        redirectInfo(item){
          this.$router.push('/adminer/socios/'+item.email+'/info');
        },
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
