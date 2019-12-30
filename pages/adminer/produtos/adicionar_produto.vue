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
      :rules="coderules"
      label="Código"
      required
    ></v-text-field>
    <v-text-field
      v-model="tipo"
      :counter="20"
      :rules="tipoRules"
      label="Tipo"
      required
    ></v-text-field>
    <v-text-field
      v-model="descricao"
      :counter="9"
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
        code: '',
        codeRules:[],

        tipo:'',
        tipoRules:[],

        descricao: '',
        descricaoRules:[],

        preco:'',
        precoRules:[
          v => !!v || 'Época é um campo obrigatório',
        ],

        stock:'',
        stockRules:[],
      }),
      methods: {

        validate () {
          if (this.$refs.form.validate()) {
            this.code = this.code + 1;
            /*this.$axios.$post('/api/inscricoes', {
              nome: this.nome,
              email: this.email,
              code: "INSC_"+this.nif,
              morada: this.morada,
              numContribuinte: this.nif,
              dataNascimento: this.selectedDate,
              numIdentificacaoCivil: this.nic,

            })
              .then(() => {
                this.$router.push('/')
              })
              .catch(error => {
                console.log(error)
              })*/
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
