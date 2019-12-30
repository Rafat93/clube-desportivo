<template>
  <div>
    <v-data-table
      :headers="headers"
      :items="produtos"
      :items-per-page="10"
      class="elevation-1"
    >
      <template slot="items" slot-scope="props">
        <tr @click="props.expanded = !props.expanded">
          <td class="text-xs">{{ props.item.code }}</td>
          <td class="text-xs">{{ props.item.tipo.nome}}</td>
          <td class="text-xs">{{ props.item.descricao}}</td>
          <td class="text-xs">{{ props.item.preco}}</td>
        </tr>
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
            text: 'Codigo',
            align: 'left',
            sortable: false,
            value: 'code'
          },
          {
            text: 'Tipo',
            align: 'left',
            sortable: false,
            value: 'tipo.nome'
          },
          {
            text: 'Descrição',
            align: 'left',
            sortable: false,
            value: 'descricao'
          },
          {
            text: 'Preço',
            align: 'left',
            sortable: false,
            value: 'preco'
          }
        ],
        produtos: []
      }
    },
    created () {

      this.$axios.$get('/api/produtos')
        .then((produtos) => {
          this.produtos = produtos;
          console.log(produtos);
        });
    }
  }
</script>

<style scoped>

</style>
