<template>
  <div>
    <v-data-table
      :headers="headers"
      :items="atletas"
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
    },
    created () {
      this.getAtletas();
    }
  }
</script>
