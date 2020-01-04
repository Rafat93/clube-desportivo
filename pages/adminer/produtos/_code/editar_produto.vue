<template>
  <v-form
    ref="form"
    v-model="valid"
    lazy-validation
  >
    <p class="subtitle-1 text-center">Edição do Produto {{this.$route.params.code}}</p>
    <v-text-field
      v-model="produto.code"
      :counter="5"
      :rules="codeRules"
      label="Code"
      disabled
      required
    ></v-text-field>
    <v-select
      v-model="produto.tipo"
      :items="all_tipo_produtos"
      label="Tipo produto"
      item-text="nome"
      item-value="nome"
      chips
      persistent-hint
    ></v-select>
    <v-text-field
      v-model="produto.descricao"
      :counter="100"
      :rules="descricaoRules"
      label="Descrição"
      required
    ></v-text-field>
    <v-text-field
      v-model="produto.preco"
      :counter="9"
      :rules="precoRules"
      label="Preço"
      required
      type="number"
    ></v-text-field>
    <v-text-field
      v-model="produto.stock"
      :counter="9"
      :rules="stockRules"
      label="Stock"
      required
      type="number"
    ></v-text-field>

    <v-btn
      :disabled="!valid"
      color="success"
      class="mr-4"
      @click="validate"
    >
      Confirmar
    </v-btn>

    <v-btn
      color="error"
      class="mr-4"
      @click="reset"
    >
      Reset Formulário
    </v-btn>

    <v-btn
      color="warning"
      class="mr-4"
      @click="resetValidation"
    >
      Reset Validação
    </v-btn>
    <v-btn
      color="error"
      class="mr-4"
      @click="cancel"
    >
      Cancelar
    </v-btn>
  </v-form>
</template>
<script>
  export default {
    name: "editar_modalidade",
    data: () => ({
      valid:true,
      produto:'',
      all_tipo_produtos:[],

      codeRules:[
        v => !!v || 'Código é um campo obrigatório',
        v => (v && v.length <= 5) || 'Código deve ter até 10 caracteres',
      ],

      tipoRules:[
        v => !!v || 'Tipo é um campo obrigatório',
      ],

      descricaoRules:[
        v => !!v || 'Código é um campo obrigatório',
      ],

      precoRules:[
        v => !!v || 'Preço é um campo obrigatório',
      ],

      stockRules:[
        v => !!v || 'Stock é um campo obrigatório',
      ],
    }),
    methods: {
      getProduto(){
        this.$axios.$get('/api/produtos/'+this.$route.params.code).then((produto) => {
          this.produto = produto;
        })
      },
      getTipoProdutos(){
        this.$axios.$get('/api/tipo_produtos')
          .then((all_tipo_produtos) => {
            this.all_tipo_produtos = all_tipo_produtos;
          });
      },
      validate () {
        if (this.$refs.form.validate()) {
          this.$axios.$put('/api/produtos/'+this.$route.params.code, {
            code: this.produto.code,
            tipo: this.produto.tipo,
            descricao: this.produto.descricao,
            preco: this.produto.preco,
            stock: this.produto.stock,
          })
            .then(() => {
              this.$router.push('/adminer/produtos/list')
            })
            .catch(error => {
              console.log(error)
            })
        }

      },
      reset () {
        this.$refs.form.reset()
      },
      resetValidation () {
        this.$refs.form.resetValidation()
      },
      cancel(){
        this.$router.push('/adminer/produtos/list')
      }
    },
    created() {
      this.getProduto();
      this.getTipoProdutos();
    }
  }
</script>
