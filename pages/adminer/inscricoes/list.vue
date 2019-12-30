<template>
  <div>
    <v-data-table
      :headers="headers"
      :items="inscricoes"
      :items-per-page="10"
      class="elevation-1"
    >
      <template v-slot:item.action="{ item }">
        <v-icon

          class="mr-2"
          @click="acceptItem(item)"
        >
          {{'mdi-check-box-outline'}}
        </v-icon>
        <v-icon

          @click="deleteItem(item)"
        >
          {{'mdi-delete'}}
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
              text: 'Codigo',
              align: 'left',
              sortable: false,
              value: 'code'
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
              sortable: false,
              value: 'numIdentificacaoCivil'
            },
            {
              text: 'Nº Identificação Fiscal',
              align: 'left',
              sortable: false,
              value: 'numContribuinte'
            },
            {
              text: 'Morada',
              align: 'left',
              sortable: false,
              value: 'morada'
            },
            { text: 'Actions', value: 'action', sortable: false },

          ],
          inscricoes: []
        }
      },
      created () {

          this.getInscricoes();

      },
      methods:{

        getInscricoes (){
          this.$axios.$get('/api/inscricoes/pendentes')
            .then((inscricoes) => {
              this.inscricoes = inscricoes;
            });
        },
        acceptItem (item) {
          this.$axios.$put('api/inscricoes/'+item.code+'/confirm').then( inscricoes =>
            this.getInscricoes(),
            this.$toast.success('Aceite!')
          );


        },

        deleteItem (item) {

          let response = confirm('Are you sure you want to delete this item?');
          if(response == true){
            this.$axios.$delete('/api/inscricoes/'+item.code);

            this.$toast.success('apagou');
            this.$toast.show(item.code );
            this.getInscricoes();

          }

        },
      }
    }
</script>

<style scoped>

</style>
