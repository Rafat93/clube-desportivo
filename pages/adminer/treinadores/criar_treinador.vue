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
      <p class="subtitle-1 text-center">Criar Treinador</p>

      <v-text-field
        v-model="nome"
        :counter="20"
        :rules="nomeRules"
        label="Nome"
        required
      ></v-text-field>
      <v-text-field
        v-model="numeroCedula"
        :counter="20"
        :rules="numeroCedulaRules"
        label="Nº Cédula"
        required
      ></v-text-field>
      <v-text-field
        v-model="email"
        :counter="20"
        :rules="emailRules"
        label="Email"
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
        name: "criar_treinador",
      data: () => ({
        valid:true,

        // ---- SNACKBAR INFO -----
        color: '',
        mode: '',
        snackbar: false,
        text: '',
        timeout: 4000,
        x: null,
        y: 'top',
        // ------------------------



        nome: '',
        nomeRules:[
          v => !!v || 'Nome é um campo obrigatório',
          v => (v && v.length <= 20 ) || 'Nome deve ter até 20 caracteres.',
        ],

        email:'',
        emailRules:[
          v => !!v || 'Email é um campo obrigatório',
          v => /.+@.+\..+/.test(v) || 'E-mail deve ser válido',
        ],
        password:'',
        password_confirmation: '',
        passwordRules:[
          v => !!v || 'Password é um campo obrigatório',
          v => (v && v.length >= 4 ) || 'Password deve ter pelo menos 4 digitos.',
        ],

        numeroCedula: '',
        numeroCedulaRules:[
          v => !!v || 'Nº Cédula é um campo obrigatório',
        ],

      }),
      methods: {

        validate() {
          if (this.$refs.form.validate()) {
            if (this.password_confirmation == this.password) {
              this.$axios.$post('/api/treinadores/', {
                nome: this.nome,
                email: this.email,
                password: this.password,
                numeroCedula: this.numeroCedula,

              })
                .then(() => {
                  this.$router.push('/adminer/treinadores/list')
                })
                .catch(error => {
                  console.log(error)
                })
            } else {
              this.color = 'red';
              this.text = 'Password e confirmação não correspondem.';
              this.snackbar = true;

            }
          }
        },
        reset() {
          this.$refs.form.reset()
        },
        resetValidation() {
          this.$refs.form.resetValidation()
        },
        cancel() {
          this.$router.push('/adminer/treinadores/list')
        }
      }
    }
</script>

<style scoped>

</style>
