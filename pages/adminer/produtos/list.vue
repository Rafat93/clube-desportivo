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
      <template v-slot:item.action="{ item }">
        <v-icon
          @click="deleteItem(item)"
        >
          {{'mdi-delete'}}
        </v-icon>
        <v-icon
          @click="editItem(item)"
        >{{'mdi-pencil'}}
        </v-icon>
      </template>
    </v-data-table>
  </div>
</template>

<script>
  export default {
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
      deleteItem (item) {
        let response = confirm('Are you sure you want to delete this item?');
        if(response == true){
          this.$axios.$delete('/api/produtos/'+item.code).then( produtos =>
            {
              this.color = 'green';
              this.text = 'Produto com o código - '+item.code+' - eliminado com sucesso!';
              this.snackbar = true;
              this.getProdutos();
            }
          );
        }
      },
      editItem(item) {
        this.$router.push('/adminer/produtos/'+item.code+'/editar_produto');
      }
    }
  }
</script>

<style scoped>

</style>
