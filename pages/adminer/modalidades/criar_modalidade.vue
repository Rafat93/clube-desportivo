<template>
  <v-form
    ref="form"
    v-model="valid"
    lazy-validation
  >
    <p class="subtitle-1 text-center">Criação de Nova Modalidade</p>
    <v-text-field
      v-model="sigla"
      :counter="5"
      :rules="siglaRules"
      label="Sigla"
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
      v-model="epoca"
      :counter="9"
      :rules="epocaRules"
      label="Época"
      hint="20XX/20XX"
      required
    ></v-text-field>
    <v-text-field
      v-model="quotaAnual"
      :counter="9"
      :rules="quotaAnualRules"
      label="Quota Anual"
      hint="10.50"
      required
      type="number"
      step="0.01"
    ></v-text-field>

    <v-btn
      :disabled="!valid"
      color="success"
      class="mr-4"
      @click="validate"
    >
      Validate
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
      Reset Validation
    </v-btn>
  </v-form>
</template>

<script>
    export default {
      name: "criar_modalidade",
      data: () => ({
        valid:true,

        sigla: '',
        siglaRules:[
          v => !!v || 'Sigla é um campo obrigatório',
          v => (v && v.length <= 5 ) || 'Sigla deve ter até 5 caracteres.',
        ],

        nome: '',
        nomeRules:[
          v => !!v || 'Nome é um campo obrigatório',
          v => (v && v.length <= 20 ) || 'Nome deve ter até 20 caracteres.',
        ],

        epoca:'',
        epocaRules:[
          v => !!v || 'Época é um campo obrigatório',
          v => (v && v.length === 9 ) || 'Época deve ter 9 caracteres. Seguindo o formato: 20XX/20XX.',
          v => /([/])/.test(v) || 'Must have one special character [/]',
        ],
        quotaAnual:'',
        quotaAnualRules:[
          v => !!v || 'Época é um campo obrigatório',
        ],
      }),
      methods: {

        validate () {
          if (this.$refs.form.validate()) {
            this.$axios.$post('/api/modalidades', {
              sigla: this.sigla,
              nome: this.nome,
              epocaDesportiva: this.epoca,
              quotaAnual: this.quotaAnual,
            })
              .then(() => {
                this.$router.push('/adminer/modalidades/list')
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
      },
    }
</script>

<style scoped>

</style>
