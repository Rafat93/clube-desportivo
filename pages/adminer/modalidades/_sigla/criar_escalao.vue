<template>
  <v-form ref="form" v-model="valid" lazy-validation>
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
      type="number"
      label="Idade Minima"
      required
    ></v-text-field>
    <v-text-field
      v-model="idadeMax"
      :counter="2"
      :rules="idadeMaxRules"
      label="Idade Máxima"
      required
      type="number"
    ></v-text-field>

    <v-btn :disabled="!valid" color="success" class="mr-4" @click="validate">
      Adicionar
    </v-btn>

    <v-btn color="error" class="mr-4" @click="reset">
      Reset Formulário
    </v-btn>

    <v-btn color="warning" class="mr-4" @click="resetValidation">
      Reset Validação
    </v-btn>
    <v-btn color="error" @click="cancel">
      Cancelar
    </v-btn>
  </v-form>
</template>

<script>
    export default {
        name: "criar_escalao",
      data: () => ({
        valid: true,
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
          v => (v && v.length <= 2) || 'Idade Mínima deve ter no máximo 2 caracteres',
        ],

        idadeMax:'',
        idadeMaxRules:[
          v => !!v || 'Idade Máxima é um campo obrigatório',
          v => (v && v.length <= 2) || 'Idade Máxima deve ter no máximo 2 caracteres',
        ],


      }),
      methods: {
        validate () {
          if (this.$refs.form.validate()) {

            this.$axios.$post('/api/escaloes/', {
              code: this.code,
              nome: this.nome,
              idadeMin: this.idadeMin,
              idadeMax: this.idadeMax,
              siglaModalidade: this.$route.params.sigla,

            })
              .then(() => {
                this.$axios.$put('/api/escaloes/'+this.code+'/modalidades/enroll/'+this.$route.params.sigla,{
                  code: this.code,
                  sigla: this.$route.params.sigla,
                }).then(()=> {
                  this.$router.push('/adminer/modalidades/'+this.$route.params.sigla+'/info');
                }).catch(error => {
                  console.log(error)
                })
              }).catch(error => {
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
          this.$router.push('/adminer/modalidades/'+this.$route.params.sigla+'/info');
        }
      }
    }
</script>

<style scoped>

</style>
