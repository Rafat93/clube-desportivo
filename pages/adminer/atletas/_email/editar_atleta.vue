<template>
  <v-form
    ref="form"
    v-model="valid"
    lazy-validation
    >
    <p class="subtitle-1 text-center">Edição do Atleta: {{this.atleta.nome}}</p>
    <v-text-field
      v-model="atleta.numeroSocio"
      :counter="10"
      label="Nº Sócio"
      required
      disabled
    ></v-text-field>
    <v-text-field
      v-model="atleta.nome"
      :counter="30"
      :rules="nomeRules"
      label="Nome"
      required
    ></v-text-field>
    <v-text-field
      v-model="atleta.email"
      :counter="30"
      label="Email"
      required
      disabled
    ></v-text-field>
    <v-menu
      v-model="showPicker"
      :close-on-content-click="false"
      transition="scale-transition"
      offset-y
      full-width
      max-width="290px"
      min-width="290px"
    >
      <template v-slot:activator="{ on }">
        <v-text-field
          v-model="atleta.dataNascimento"
          label="Data de Nascimento:"
          hint="YYYY/MM/DD"
          persistent-hint
          readonly
          v-on="on"
        ></v-text-field>
      </template>
      <v-date-picker
        v-model="selectedDate"
        no-title
        @input="showPicker = false"
        :format="formatDate"
      ></v-date-picker>

    </v-menu>
    <v-text-field
      v-model="atleta.numIdentificacaoCivil"
      :counter="9"
      :rules="numIdentificacaoCivilRules"
      label="Nº Identificação Civil"
      required
    ></v-text-field>
    <v-text-field
      v-model="atleta.numContribuinte"
      :counter="9"
      :rules="numIdentificacaoCivilRules"
      label="Nº Identificação Civil"
      required
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
    name: "editar_atleta",
    data: () => ({
      valid:true,
      atleta:'',
      old_password:'',
      showPicker: false,
      selectedDate: '',

    }),
    methods: {
      formatDate (date) {
        return moment(date).format('DD-MM-YYYY')
      },
      getAtleta(){
        this.$axios.$get('/api/atletas/'+this.$route.params.email+'/').then((atleta) => {
          this.atleta = atleta;
          this.old_password = atleta.password;
        });
      },
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
      cancel(){
        this.$router.push('/adminer/atletas/'+this.$router.params.email+"/info")
      }
    },
    created() {
      this.getAtleta();
    }
  }
</script>
