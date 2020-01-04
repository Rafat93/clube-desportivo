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
        <v-btn small style="margin-bottom: -50px;" @click="redirectCriar"><v-icon>{{'mdi-plus'}}</v-icon>Atleta</v-btn>
      </v-col>
    </v-row>
    <v-data-table
      :headers="headers"
      :items="atletas"
      :search="search"
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
  </div>

</template>

<script>
  export default {
    data () {
      return {
        search:'',
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
          },
          { text: 'Actions', value: 'action', sortable: false },
        ],
        atletas: []
      }
    },
    methods:{
      redirectInfo(item){
        this.$router.push('/adminer/atletas/'+item.email+'/info');
      },
      getAtletas(){
        this.$axios.$get('/api/atletas')
          .then((atletas) => {
            this.atletas = atletas
          });
      },
      redirectCriar(){
        this.$router.push('/adminer/atletas/criar_atleta');
      },
    },
    created () {
      this.getAtletas();
    }
  }
</script>
