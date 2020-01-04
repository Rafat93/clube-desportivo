<template>
  <v-form
    ref="form"
    v-model="valid"
    lazy-validation
  >
    <p class="subtitle-1 text-center">Criação de Novo Treino</p>
    <v-text-field
      v-model="code"
      :counter="10"
      :rules="codeRules"
      label="Código"
      required
    ></v-text-field>
    <v-select
      v-model="treinadorSelecionado"
      :items="treinadoresModalidade"
      label="Treinador"
      item-text="nome"
      item-value="email"
      chips
      persistent-hint
    ></v-select>
    <v-select
      v-model="graduacaoSelecionado"
      :items="graduacoes"
      label="Graduação"
      item-text="nome"
      item-value="code"
      chips
      persistent-hint
    ></v-select>
    <v-select
      v-model="escalaoSelecionado"
      :items="escaloes"
      label="Escalão"
      item-text="nome"
      item-value="code"
      chips
      persistent-hint
    ></v-select>
    <v-text-field
      v-model="horaInicio"
      :counter="5"
      label="Hora Início"
      required
      type="time"
    ></v-text-field>
    <v-text-field
      v-model="horaFim"
      :counter="5"
      label="Hora Início"
      required
      type="time"
    ></v-text-field>
    <v-select
      v-model="diaSemSelecionado"
      :items="diasSemana"
      label="Dia de Semana"
      item-text="v"
      item-value="v"
      chips
      persistent-hint
    ></v-select>

    treinador:{{treinadorSelecionado}}
    <br>
    code:{{code}}
    <br>
    horaI: {{horaInicio}}
    <br>
    horaF: {{horaFim}}
    <br>
    grad: {{graduacaoSelecionado}}

    esc: {{escalaoSelecionado}}


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

        graduacaoSelecionado: '',
        graduacoes:[],

        escalaoSelecionado: '',
        escaloes:[],

        horaInicio: '',
        horaFim: '',

        diaSemSelecionado: '',
        diasSemana:[
          {v:"SEGUNDA"},{v:"TERCA"},{v:"QUARTA"},
          {v:"QUINTA"},{v:"SEXTA"},{v:"SABADO"}
          ,{v:"DOMINGO"}
        ],

        code: '',
        codeRules:[
          v => !!v || 'Code é um campo obrigatório',
          v => (v && v.length <= 10 ) || 'Code deve ter até 10 caracteres.',
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
            console.log("passou aqui.");
            this.$axios.$post('/api/treinos/', {
              code: this.code,
              emailTreinador: this.treinadorSelecionado,
              siglaModalidade: this.$route.params.sigla,
              codeGraduacao: this.graduacaoSelecionado,
              codeEscalao: this.escalaoSelecionado,
              horaInicio: this.horaInicio,
              horaFim: this.horaFim,
              diaSemana: this.diaSemSelecionado
            })
              .then(() => {
                this.$axios.$put("/api/treinos/"+this.code+"/modalidade/enroll/"+this.$route.params.sigla, {
                  code: this.code,
                  siglaModalidade: this.$route.params.sigla,
                }).then(() => {
                  this.$router.push('/adminer/modalidades/'+this.$route.params.sigla+'/info')
                }).catch(error => {
                  console.log(error)
                })
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
        },
        getEscaloes(){
          this.$axios.$get('/api/modalidades/'+this.$route.params.sigla+'/escaloes/').then((escaloes) => {
            console.log("escaloes: "+escaloes);
            this.escaloes = escaloes;
          });
        },
        getGraduacoes(){
          this.$axios.$get('/api/modalidades/'+this.$route.params.sigla+'/graduacoes/').then((graduacoes) => {
            console.log("Graduacoes"+graduacoes);
            this.graduacoes = graduacoes;
          });
        },
      },
      created() {
          this.getTreinadoresModalidade();
          this.getEscaloes();
          this.getGraduacoes();
      }
    }
</script>

<style scoped>

</style>
