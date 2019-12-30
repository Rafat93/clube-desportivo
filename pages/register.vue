<template>
  <v-form
    ref="form"
    v-model="valid"
    lazy-validation
  >
    <p class="subtitle-1 text-center">Inscrição de Sócio/Atleta</p>
    <v-text-field
      v-model="nome"
      :counter="30"
      :rules="nameRules"
      label="Nome"
      required
    ></v-text-field>

    <v-text-field
      v-model="email"
      :rules="emailRules"
      label="E-mail"
      required
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
        v-model="selectedDate"
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
    ></v-date-picker>
    </v-menu>

    <v-text-field
      v-model="nif"
      :rules="nifRules"
      label="NIF"
      :counter="9"
      type="number"
      required
    ></v-text-field>

    <v-text-field
      v-model="nic"
      :rules="nicRules"
      label="Nº Identificação Civil"
      :counter="9"
      type="number"
      required
    ></v-text-field>

    <v-text-field
      v-model="morada"
      :rules="moradaRules"
      label="Morada"
      :counter="80"
      required
    ></v-text-field>


    <v-col cols="8">
      <v-text-field
        label="Quota anual:"
        value="12.00"
        prefix="€"
        filled
        readonly
      ></v-text-field>
    </v-col>


    <v-checkbox
      v-model="checkbox"
      :label="`Atleta `"
    ></v-checkbox>

    {{selectedDate}}

    <!--<div v-if="this.checkbox === true">



    </div>-->

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
      name: "register",
      data: () => ({
        valid: true,
        checkbox: true,
        showPicker: false,
        selectedDate: null,

        morada: '',
        moradaRules:[
          v => !!v || 'Morada é um campo obrigatório',
          v => (v && v.length <= 80 ) || 'Morada deve ter menos de 80 caracteres.',],

        nif: '',
        nifRules:[
          v => !!v || 'NIF é um campo obrigatório',
          v => (v && v.length ===  9) || 'NIF deve ter 9 algarismos. ',],

        nic: '',
        nicRules:[
          v => !!v || 'Nº Identificação Civil é um campo obrigatório',
          v => (v && v.length ===  9) || 'Nº Identificação Civil deve ter 9 algarismos. ',],

        nome: '',
        nomeRules: [
          v => !!v || 'Name é um campo obrigatório',
          v => (v && v.length <= 30) || 'Nome deve ter até 30 caracteres',
        ],

        email: '',
        emailRules: [
          v => !!v || 'E-mail is required',
          v => /.+@.+\..+/.test(v) || 'E-mail must be valid',
        ],
        code: 0,

      }),
      methods: {
        validate () {
          if (this.$refs.form.validate()) {
            this.code = this.code + 1;
            this.$axios.$post('/api/inscricoes', {
              nome: this.nome,
              email: this.email,
              code: this.code,
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
