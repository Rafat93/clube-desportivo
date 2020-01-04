<template>
  <div>
    <v-snackbar
      v-model="snackbar"
      :bottom="y === 'bottom'"
      :color="color"
      :left="x === 'left'"
      :multi-line="mode === 'multi-line'"

      :timeout="timeout"
      :top="y === 'top'"
      :vertical="mode === 'vertical'"
    >
      {{ text }}
      <v-btn dark text @click="snackbar = false">
        Close
      </v-btn>
    </v-snackbar>
    <v-form
      ref="form"
      v-model="valid"
      lazy-validation
    >
      <p class="subtitle-1 text-center">Criação de Atleta</p>
      <v-text-field
        v-model="nome"
        :counter="30"
        :rules="nomeRules"
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
          :format="formatDate"
          :max="getEndDate"
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
      <v-text-field
        v-model="password"
        :counter="20"
        :rules="passwordRules"
        type="password"
        label="Password"
        required
      ></v-text-field>
      <v-text-field
        v-model="password_confirmation"
        :counter="20"
        :rules="passwordRules"
        label="Confirmação de Password"
        type="password"
        required
      ></v-text-field>


      <v-btn
        :disabled="!valid"
        color="success"
        class="mr-4"
        @click="validate"
      >
        Submeter
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
  </div>
</template>

<script>
  export default {
    name: "criar_socio",
    data: () => ({
      valid: true,
      date: new Date(),

      showPicker: false,
      selectedDate: '',
      numeroSocios:'',


      // ---- SNACKBAR INFO -----
      color: '',
      mode: '',
      snackbar: false,
      text: '',
      timeout: 4000,
      x: null,
      y: 'top',
      // ------------------------

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
        v => !!v || 'Email é um campo obrigatório',
        v => /.+@.+\..+/.test(v) || 'E-mail deve ser válido',
      ],
      password:'',
      password_confirmation: '',
      passwordRules:[
        v => !!v || 'Password é um campo obrigatório',
        v => (v && v.length >= 4 ) || 'Password deve ter pelo menos 4 digitos.',
      ],

    }),

    methods: {
      formatDate (date) {
        return moment(date).format('DD-MM-YYYY')
      },
      validate () {
        if (this.$refs.form.validate()) {
          if (this.password_confirmation == this.password) {
            this.$axios.$post('/api/socios/', {
              numeroSocio: this.numeroSocios + 1,
              nome: this.nome,
              password: this.password,
              email: this.email,
              dataNascimento: this.selectedDate,
              numIdentificacaoCivil: this.nic,
              numContribuinte: this.nif,
              morada: this.morada,
            })
              .then(() => {
                this.$router.push('/adminer/socios/list');
              })
              .catch(error => {
                console.log(error)
              })
          }else {
            this.color = 'red';
            this.text = 'Password e confirmação não correspondem.';
            this.snackbar = true;

          }
        }

      },
      reset () {
        this.$refs.form.reset()
      },
      resetValidation () {
        this.$refs.form.resetValidation()
      },
      cancel(){
        this.$router.push('/adminer/socios/list')
      },
      getSocios(){
        this.$axios.$get('/api/socios/')
          .then((socios) => {
            this.numeroSocios = socios.length;
          });
      }
    },
    created() {
      this.getSocios();
    },
    computed:{
      getEndDate(){
        var endDate = new Date(this.date.getFullYear(), this.date.getMonth(), this.date.getDay()-2);
        return endDate.toISOString().slice(0,10)
      },
    }
  }
</script>

<style scoped>

</style>
