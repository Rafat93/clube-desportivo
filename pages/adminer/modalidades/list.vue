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
        <v-btn small style="margin-bottom: -50px;" to="/adminer/modalidades/criar_modalidade"  ><v-icon>{{'mdi-plus'}}</v-icon>Modalidade</v-btn>
      </v-col>
    </v-row>
    <v-data-table
      :headers="headers"
      :items="modalidades"
      :search="search"
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
    data () {
      return {
        search:'',
        headers: [
          {
            text: 'Sigla',
            align: 'center',
            sortable: false,
            value: 'sigla'
          },
          {
            text: 'Nome',
            align: 'center',
            sortable: false,
            value: 'nome'
          },
          {
            text: 'Época Desportiva',
            align: 'center',
            sortable: false,
            value: 'epocaDesportiva'
          },
          {
            text: 'Quota Anual (€)',
            align: 'center',
            sortable: false,
            value: 'quotaAnual'
          },
          { text: 'Actions', value: 'action', sortable: false },

        ],
        modalidades: []
      }
    },
    methods:{
      redirectInfo(item){
        this.$router.push('/adminer/modalidades/'+item.sigla+'/info');
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
