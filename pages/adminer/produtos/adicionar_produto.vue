<template>
  <v-form
    ref="form"
    v-model="valid"
    lazy-validation
  >
    <p class="subtitle-1 text-center">Adicionar Novo Produto</p>
    <v-text-field
      v-model="code"
      :counter="5"
      :rules="codeRules"
      label="Código"
      required
    ></v-text-field>
    <v-select
      v-model="tipo"
      :items="all_tipo_produtos"
      label="Selecione um tipo de produto."
      item-text="nome"
      item-value="nome"
      chips
      persistent-hint
    ></v-select>
    <v-text-field
      v-model="descricao"
      :counter="50"
      :rules="descricaoRules"
      label="Descrição"
      required
    ></v-text-field>
    <v-text-field
      v-model="preco"
      :counter="9"
      :rules="precoRules"
      type="number"
      label="Preço"
      prefix="€"
      required
    ></v-text-field>
    <v-text-field
      v-model="stock"
      :counter="9"
      type="number"
      :rules="stockRules"
      label="Stock"
      required
    ></v-text-field>
    <v-btn
      :disabled="!valid"
      color="success"
      class="mr-4"
      @click="validate"
    >
      Adicionar
    </v-btn>

    <v-btn
      color="error"
      class="mr-4"
      @click="reset"
    >
      Reset Form
    </v-btn>

    <v-btn
      color="warning"
      @click="resetValidation"
    >
      Reset Validação
    </v-btn>
  </v-form>
</template>

<script>
    export default {
        name: "adicionar_produto",
      data: () => ({
        valid: true,
        all_tipo_produtos:[],

        code: '',
        codeRules:[
          v => !!v || 'Código é um campo obrigatório',
          v => (v && v.length <= 5) || 'Código deve ter até 10 caracteres',
        ],

        tipo:'',
        tipoRules:[
          v => !!v || 'Tipo é um campo obrigatório',
        ],

        descricao: '',
        descricaoRules:[],

        preco:0,
        precoRules:[
          v => !!v || 'Preço é um campo obrigatório',
        ],

        stock:0,
        stockRules:[
          v => !!v || 'Stock é um campo obrigatório',
        ],
      }),
      created() {
          this.getTipoProdutos();
      },
      methods: {
        getTipoProdutos(){
          this.$axios.$get('/api/tipo_produtos')
          .then((all_tipo_produtos) => {
            this.all_tipo_produtos = all_tipo_produtos;
          });
        },
        validate () {
          if (this.$refs.form.validate()) {
            this.$axios.$post('/api/produtos/', {
              code: this.code,
              tipo: this.tipo,
              descricao: this.descricao,
              preco: this.preco,
              stock: this.stock,
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
        }
      }
    }
</script>

<style scoped>

</style>
