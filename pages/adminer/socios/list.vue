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
      <v-col align="center">
        <v-btn small style="margin-bottom: -50px;" @click="redirectCriar"><v-icon>{{'mdi-plus'}}</v-icon>Sócio</v-btn>
      </v-col>
    </v-row>
    <v-data-table
      :headers="headers"
      :items="socios"
      :search="search"
      :items-per-page="10"
      class="elevation-1"
    >
      <template v-slot:item.action="{ item }" >
        <v-icon @click="redirectInfo(item)">
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
          search:'',
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
        redirectCriar(item){
          this.$router.push('/adminer/socios/criar_socio');
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
