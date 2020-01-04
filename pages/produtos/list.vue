<template>
  <div>
    <v-snackbar
      v-model="snackbar"
      :bottom="y === 'bottom'"
      :color="color"
      :left="x === 'left'"
      :multi-line="mode === 'multi-line'"

      :timeout="timeout"
      :top="y === 'top'"
      :vertical="mode === 'vertical'"
    >
      {{ text }}
      <v-btn dark text @click="snackbar = false">
        Close
      </v-btn>
    </v-snackbar>
    <v-data-table
      :headers="headers"
      :items="produtos"
      :items-per-page="10"
      class="elevation-1"
    >
      <template v-slot:item.action="{ item }">
        <v-icon
          @click="deleteItem(item)"
        >
          {{'mdi-plus'}}
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
          color: '',
          mode: '',
          snackbar: false,
          text: '',
          timeout: 4000,
          x: null,
          y: 'top',
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
              value: 'tipo'
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
            },
            {
              text: 'Stock',
              align: 'left',
              sortable: false,
              value: 'stock'
            },
            { text: 'Actions', value: 'action', sortable: false },
          ],
          produtos: []
        }
      },
      created () {

        this.getProdutos();
      },
      methods:{

        getProdutos (){
          this.$axios.$get('/api/produtos')
            .then((produtos) => {
              this.produtos = produtos;
            });
        },

      }
    }
</script>

<style scoped>

</style>
