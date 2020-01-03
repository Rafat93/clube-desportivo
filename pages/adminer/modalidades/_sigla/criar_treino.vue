<template>
  <v-form
    ref="form"
    v-model="valid"
    lazy-validation
  >
    <p class="subtitle-1 text-center">Criação de Novo Treino</p>
    <v-text-field
      v-model="code"
      :counter="5"
      :rules="codeRules"
      label="Código"
      required
    ></v-text-field>
    <v-select
      v-model="treinadorSelecionado"
      :items="treinadoresModalidade"
      label="Selecione"
      item-text="nome"
      item-value="email"
      chips
      hint="Selecione um treinador."
      persistent-hint
    ></v-select>




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
        name: "criar_treino",
      data: () => ({
        valid:true,

        treinadoresModalidade:[],
        treinadorSelecionado: '',

        code: '',
        codeRules:[
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
      }),
      methods: {

        validate () {
          if (this.$refs.form.validate()) {
            this.$axios.$post('/api/modalidades', {
              sigla: this.sigla,
              nome: this.nome,
              epocaDesportiva: this.epoca,
            })
              .then(() => {
                this.$router.push('adminer/modalidades/list')
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
          this.$router.push('/adminer/modalidades/'+this.$route.params.sigla+'/info')
        },
        getTreinadoresModalidade(){
          this.$axios.$get('/api/modalidades/'+this.$route.params.sigla+'/treinadores').then((treinadores) => {
            this.treinadoresModalidade = treinadores;
          });
        }
      },
      created() {
          this.getTreinadoresModalidade();
      }
    }
</script>

<style scoped>

</style>
