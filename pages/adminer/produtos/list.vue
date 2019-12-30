<template>
  <div>
    <v-row>
      <v-col>
        <v-btn color="primary" to="/adminer/produtos/adicionar_produto" >Adicionar Produto</v-btn>
      </v-col>
    </v-row>
    <v-data-table
      :headers="headers"
      :items="produtos"
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
</template>

<script>
    export default {
      name: "list",
      data () {
        return {
          headers: [
            {
              text: 'Code',
              align: 'left',
              sortable: false,
              value: 'code'
            },
            {
              text: 'Descrição',
              align: 'left',
              sortable: false,
              value: 'descricao'
            },
            {
              text: 'Preço (€)',
              align: 'left',
              sortable: false,
              value: 'precoEmEuros',
            },
            {
              text: 'Stock',
              align: 'left',
              sortable: false,
              value: 'stock',
            },

          ],
          produtos: []
        }
      },
      created () {

        this.$axios.$get('/api/produtos')
          .then((produtos) => {
            this.produtos = produtos;
          });
      }
    }
</script>

<style scoped>

</style>
