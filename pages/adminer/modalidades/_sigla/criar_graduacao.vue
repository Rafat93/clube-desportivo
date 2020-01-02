<template>
  <v-form
    ref="form"
    v-model="valid"
    lazy-validation
  >
    <p class="subtitle-1 text-center">Criação de Graduação</p>
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
        name: "criar_graduacao",
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

      }),
      methods: {
        validate () {
          if (this.$refs.form.validate()) {

            this.$axios.$post('/api/graduacoes/', {
              code: this.code,
              nome: this.nome,
              siglaModalidade: this.$route.params.sigla

            })
              .then(() => {
                this.$router.push('/adminer/modalidades/'+this.$route.params.sigla+'/info')
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
