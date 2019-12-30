<template>
  <v-form
    ref="form"
    v-model="valid"
    lazy-validation
  >
    <p class="subtitle-1 text-center">Criação de Novo Escalão</p>
    <v-text-field
      v-model="code"
      :counter="10"
      :rules="codeRules"
      label="Código"
      required
    ></v-text-field>
    <v-text-field
      v-model="nome"
      :counter="20"
      :rules="nomeRules"
      label="Nome"
      required
    ></v-text-field>
    <v-text-field
      v-model="idadeMin"
      :counter="2"
      :rules="idadeMinRules"
      label="Idade Minima"
      required
    ></v-text-field>
    <v-text-field
      v-model="idadeMax"
      :counter="2"
      :rules="idadeMaxRules"
      label="Idade Máxima"
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
      Reset Formulário
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
        name: "criar_escalao",
      data: () => ({
        code: '',
        codeRules:[
          v => !!v || 'Código é um campo obrigatório',
          v => (v && v.length <= 10) || 'Código deve ter até 10 caracteres',
        ],

        nome: '',
        nomeRules:[
          v => !!v || 'Nome é um campo obrigatório',
          v => (v && v.length <= 20) || 'Nome deve ter até 20 caracteres',
        ],

        idadeMin:'',
        idadeMinRules:[
          v => !!v || 'Idade Mínima é um campo obrigatório',
          v => (v && v.length === 2) || 'Idade Mínima deve ter 2 caracteres',
        ],

        idadeMax:'',
        idadeMaxRules:[
          v => !!v || 'Idade Máxima é um campo obrigatório',
          v => (v && v.length === 2) || 'Idade Máxima deve ter 2 caracteres',
        ],

        stock:'',
        stockRules:[],
      }),
      methods: {
        validate () {
          if (this.$refs.form.validate()) {

            this.$axios.$post('/api/inscricoes', {
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
