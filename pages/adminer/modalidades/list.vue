<template>
  <div>
    <div>
      <v-row>
        <v-col>
          <v-btn color="primary" to="/adminer/modalidades/criar_modalidade" >Criar Modalidade</v-btn>
        </v-col>
        <v-col>
          <v-btn color="primary">Primary</v-btn>
        </v-col>
        <v-col>
          <v-btn color="primary">Primary</v-btn>
        </v-col>
        <v-col>
          <v-btn color="primary">Primary</v-btn>
        </v-col>
      </v-row>
      <v-data-table
        :headers="headers"
        :items="modalidades"
        :items-per-page="10"
        class="elevation-1"
      >
        <template slot="items" slot-scope="props">
          <tr @click="props.expanded = !props.expanded">
            <td class="text-xs">{{ props.item.nome }}</td>
            <td class="text-xs">{{ props.item.email}}</td>
          </tr>
        </template>
      </v-data-table>
    </div>
  </div>
</template>

<script>

  export default {
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

        ],
        modalidades: []
      }
    },
    created () {

      this.$axios.$get('/api/modalidades')
        .then((modalidades) => {
          this.modalidades = modalidades;
          console.log(modalidades);
        });


    }
  }
</script>

<style scoped>

</style>
