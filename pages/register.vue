<template>
  <v-form
    ref="form"
    v-model="valid"
    lazy-validation
  >
    <v-text-field
      v-model="name"
      :counter="10"
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
        showPicker: false,
        selectedDate: null,

        name: '',
        nameRules: [
          v => !!v || 'Name is required',
          v => (v && v.length <= 10) || 'Name must be less than 10 characters',
        ],
        email: '',
        emailRules: [
          v => !!v || 'E-mail is required',
          v => /.+@.+\..+/.test(v) || 'E-mail must be valid',
        ],

      }),
      methods: {

        validate () {
          if (this.$refs.form.validate()) {
            this.snackbar = true
          }
        },
        reset () {
          this.$refs.form.reset()
        },
        resetValidation () {
          this.$refs.form.resetValidation()
        },
      },


    }
</script>

<style scoped>

</style>
