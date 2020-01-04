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
        <v-dialog v-model="dialog_tipo_produto" width="500">
          <v-card>
            <v-card-title class="headline grey lighten-2" primary-title>
              Criar Tipo Produto
            </v-card-title>

            <v-card-text>
              Insira o nome do novo Tipo de Produto:
              <v-text-field
                v-model="nome"
                :counter="20"
                :rules="nomeRules"
                label="Nome"
                required
              ></v-text-field>
            </v-card-text>
            <v-divider></v-divider>

            <v-card-actions>
              <v-spacer></v-spacer>
              <v-btn color="primary" text @click="criarTipoProduto">
                Aceitar
              </v-btn>
              <v-btn color="error" text @click="dialog_tipo_produto = false">
                Cancelar
              </v-btn>
            </v-card-actions>
          </v-card>
          <template v-slot:activator="{ on }">
            <v-btn small color="primary" v-on="on" width="130px;" style="margin-bottom: 5px;" ><v-icon>{{'mdi-plus'}}</v-icon>Tipo Produto</v-btn>
          </template>
        </v-dialog>
      </v-col>
    </v-row>
    <v-data-table
      :headers="headers"
      :items="tipo_produtos"
      :items-per-page="10"
      :search="search"
      class="elevation-1"
    >
      <template v-slot:item.action="{ item }">
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
    data () {
      return {
        search: '',
        color: '',
        mode: '',
        snackbar: false,
        text: '',
        timeout: 4000,
        x: null,
        y: 'top',
        headers: [
          {
            text: 'Nome',
            align: 'left',
            sortable: false,
            value: 'nome'
          },
          { text: 'Actions', value: 'action', sortable: false },
        ],
        tipo_produtos: [],

        nome:'',
        nomeRules: [
          v => !!v || 'Nome é um campo obrigatório',
          v => (v && v.length <= 20) || 'Nome deve ter até 20 caracteres',
        ],
        dialog_tipo_produto: false,
      }
    },
    created () {

      this.getTipoProdutos();
    },
    methods:{
      criarTipoProduto(){
        this.$axios.$post('/api/tipo_produtos',{
          nome: this.nome,
        }).then(()=> {
            this.getTipoProdutos();
            this.dialog_tipo_produto = false;
            this.nome = '';
          })
      },
      getTipoProdutos (){
        this.$axios.$get('/api/tipo_produtos')
          .then((tipo_produtos) => {
            this.tipo_produtos = tipo_produtos;
          });
      },
      deleteItem (item) {
        let response = confirm('Are you sure you want to delete this item?');
        if(response == true){
          this.$axios.$delete('/api/tipo_produtos/'+item.nome).then( tipo_produtos =>
            {
              this.color = 'green';
              this.text = 'Tipo Produto com o nome - '+item.nome+' - eliminado com sucesso!';
              this.snackbar = true;
              this.getTipoProdutos();
            }
          ).catch(error => {
            console.log(error);
            this.color = 'red';
            this.text = 'Tipo Produto com o nome - '+item.nome+' - está associado a Produtos. IMPOSSIVEL ELIMINAR';
            this.snackbar = true;
          });
        }
      },
      editItem(item) {
        //this.$router.push('/adminer/produtos/'+item.code+'/editar_produto');
      }
    }
  }
</script>

<style scoped>

</style>
