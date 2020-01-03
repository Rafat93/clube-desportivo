<template>
  <v-form
    ref="form"
    v-model="valid"
    lazy-validation
  >
    <p class="subtitle-1 text-center">Criação de Nova Modalidade</p>
    <v-text-field
      v-model="modalidade.sigla"
      :counter="5"
      :rules="siglaRules"
      label="Sigla"
      disabled
      required
    ></v-text-field>
    <v-text-field
      v-model="modalidade.nome"
      :counter="20"
      :rules="nomeRules"
      label="Nome"
      required
    ></v-text-field>
    <v-text-field
      v-model="modalidade.epocaDesportiva"
      :counter="9"
      :rules="epocaRules"
      label="Época"
      hint="20XX/20XX"
      required
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
        name: "editar_modalidade",
      data: () => ({
        valid:true,
        modalidade:'',


        siglaRules:[
          v => !!v || 'Sigla é um campo obrigatório',
          v => (v && v.length <= 5 ) || 'Sigla deve ter até 5 caracteres.',
        ],


        nomeRules:[
          v => !!v || 'Nome é um campo obrigatório',
          v => (v && v.length <= 20 ) || 'Nome deve ter até 20 caracteres.',
        ],


        epocaRules:[
          v => !!v || 'Época é um campo obrigatório',
          v => (v && v.length === 9 ) || 'Época deve ter 9 caracteres. Seguindo o formato: 20XX/20XX.',
          v => /([/])/.test(v) || 'Must have one special character [/]',
        ],
      }),
      methods: {

        validate () {
          if (this.$refs.form.validate()) {
            this.$axios.$put('/api/modalidades/'+this.$route.params.sigla, {
              sigla: this.modalidade.sigla,
              nome: this.modalidade.nome,
              epocaDesportiva: this.modalidade.epocaDesportiva,
            })
              .then(() => {
                this.$router.push('/adminer/modalidades/'+this.modalidade.sigla+'/info');
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
        getModalidade(){
          this.$axios.$get('/api/modalidades/'+this.$route.params.sigla).then((modalidade) => {
              this.modalidade = modalidade;
            })
        },
      },
      created() {
          this.getModalidade();
      }
    }
</script>

<style scoped>

</style>
