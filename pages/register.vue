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
      <p class="subtitle-1 text-center">Inscrição de Sócio/Atleta</p>
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


      <v-col cols="8">
        <v-text-field
          label="Quota anual:"
          v-model="valorQuotaAnual"
          prefix="€"
          filled
          readonly
        ></v-text-field>
      </v-col>

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
  </div>

</template>

<script>
    export default {
      name: "register",
      data: () => ({
        valid: true,
        checkbox: false,
        date: new Date(),

        valorQuotaAnual: 12,

        showPicker: false,
        selectedDate: '',

        modalidades:[],
        checkboxes:[],

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
          v => !!v || 'E-mail is required',
          v => /.+@.+\..+/.test(v) || 'E-mail must be valid',
        ],

      }),

      methods: {
        formatDate (date) {
          return moment(date).format('DD-MM-YYYY')
        },
        getModalidades (){
          this.$axios.$get('/api/modalidades/').then((modalidades) => {
            this.modalidades = modalidades;
          });
        },
        validate () {
          if (this.$refs.form.validate()) {
            this.$axios.$post('/api/inscricoes', {
              nome: this.nome,
              email: this.email,
              code: "INSC_"+this.nif,
              morada: this.morada,
              numContribuinte: this.nif,
              dataNascimento: this.selectedDate,
              numIdentificacaoCivil: this.nic,

            })
              .then(() => {
                this.color = 'green';
                this.text = 'Inscrição submetida com sucesso! A sua inscrição ficará a aguardar aceitação. ' +
                  'Receberá novas informações no seu email.';
                this.snackbar = true;
                setTimeout(() => {
                  this.$router.push('/');
                }, 5000);

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
      },
      created() {
        this.getModalidades();
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
