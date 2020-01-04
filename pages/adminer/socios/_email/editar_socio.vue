<template>
  <v-form
    ref="form"
    v-model="valid"
    lazy-validation
  >
    <p class="subtitle-1 text-center">Edição do Socio: {{this.socio.nome}}</p>
    <v-text-field
      v-model="socio.numeroSocio"
      :counter="10"
      label="Nº Sócio"
      required
      disabled
    ></v-text-field>
    <v-text-field
      v-model="socio.nome"
      :counter="30"
      :rules="nomeRules"
      label="Nome"
      required
    ></v-text-field>
    <v-text-field
      v-model="socio.email"
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
          v-model="socio.dataNascimento"
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
      v-model="socio.numIdentificacaoCivil"
      :counter="9"
      :rules="nicRules"
      label="Nº Identificação Civil"
      required
    ></v-text-field>
    <v-text-field
      v-model="socio.numContribuinte"
      :counter="9"
      :rules="nifRules"
      label="Nº Identificação Civil"
      required
    ></v-text-field>
    <v-text-field
      v-model="socio.morada"
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
    name: "editar_socio",
    data: () => ({
      valid:true,
      socio:'',
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
      getSocio(){
        this.$axios.$get('/api/socios/'+this.$route.params.email+'/').then((socio) => {
          this.socio = socio;
          this.old_password = socio.password;
        });
      },
      validate () {
        if (this.$refs.form.validate()) {
          if(this.selectedDate === ''){
            this.selectedDate = this.socio.dataNascimento;
          }
          this.$axios.$put('/api/socios/'+this.$route.params.email, {
            numeroSocio: this.socio.numeroSocio,
            nome: this.socio.nome,
            email: this.socio.email,
            password: this.socio.password,
            dataNascimento: this.selectedDate,
            numIdentificacaoCivil: this.socio.numIdentificacaoCivil,
            numContribuinte: this.socio.numContribuinte,
            morada: this.socio.morada,
          })
            .then(() => {
              this.$router.push('/adminer/socios/'+this.$route.params.email+'/info');
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
        this.$router.push('/adminer/socios/'+this.$route.params.email+"/info")
      }
    },
    created() {
      this.getSocio();
    }
  }
</script>
