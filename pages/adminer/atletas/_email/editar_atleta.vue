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
      :rules="nicRules"
      label="Nº Identificação Civil"
      required
    ></v-text-field>
    <v-text-field
      v-model="atleta.numContribuinte"
      :rules="nifRules"
      :counter="9"
      label="Nº Identificação Civil"
      required
    ></v-text-field>
    <v-text-field
      v-model="atleta.morada"
      :rules="moradaRules"
      label="Morada"
      :counter="80"
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
      valid: true,
      atleta:'',
      old_password:'',
      showPicker: false,
      selectedDate: '',

      nomeRules:[
        v => !!v || 'Nome é um campo obrigatório',
        v => (v && v.length <= 20 ) || 'Nome deve ter até 20 caracteres.',
      ],
      nifRules:[
        v => !!v || 'NIF é um campo obrigatório',
        v => (v && v.length ===  9) || 'NIF deve ter 9 algarismos. ',
      ],
      nicRules:[
        v => !!v || 'Nº Identificação Civil é um campo obrigatório',
        v => (v && v.length ===  9) || 'Nº Identificação Civil deve ter 9 algarismos. ',
      ],
      moradaRules:[
        v => !!v || 'Morada é um campo obrigatório',
        v => (v && v.length <= 80 ) || 'Morada deve ter menos de 80 caracteres.',
      ],
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
          console.log("data nascimento: "+this.selectedDate);
          if(this.selectedDate === ''){
            this.selectedDate = this.atleta.dataNascimento;
          }
          this.$axios.$put('/api/atletas/'+this.$route.params.email, {
            numeroSocio: this.atleta.numeroSocio,
            nome: this.atleta.nome,
            email: this.atleta.email,
            password: this.atleta.password,
            dataNascimento: this.selectedDate,
            numIdentificacaoCivil: this.atleta.numIdentificacaoCivil,
            numContribuinte: this.atleta.numContribuinte,
            morada: this.atleta.morada,
          })
            .then(() => {
              this.$router.push('/adminer/atletas/'+this.$route.params.email+'/info');
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
        this.$router.push('/adminer/atletas/'+this.$route.params.email+"/info")
      }
    },
    created() {
      this.getAtleta();
    }
  }
</script>
