<template>
  <div>
    <v-card style="margin-bottom: 10px;">
      <v-card-title class="justify-center">
        Editar perfil
      </v-card-title>
    </v-card>

    <div >
      <v-form
        ref="form"
        v-model="valid"
        lazy-validation
      >
        <p class="subtitle-1 text-center">Edição do Socio:</p>
        <v-text-field
          v-model="user.numeroSocio"
          :counter="10"
          label="Nº Sócio"
          required
          disabled
          v-if="this.$auth.user.groups == 'Atleta' || this.$auth.user.groups == 'Socio'"
        ></v-text-field>
        <v-text-field
          v-model="user.numeroCedula"
          :counter="10"
          label="Nº Cédula"
          required
          v-if="this.$auth.user.groups == 'Treinador'"
        ></v-text-field>
        <v-text-field
          v-model="user.nome"
          :counter="30"
          :rules="nomeRules"
          label="Nome"
          required
        ></v-text-field>
        <v-text-field
          v-model="user.email"
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
          v-if="this.$auth.user.groups == 'Atleta' || this.$auth.user.groups == 'Socio'"
        >
          <template v-slot:activator="{ on }">
            <v-text-field
              v-model="user.dataNascimento"
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
          v-model="user.numIdentificacaoCivil"
          :counter="9"
          :rules="numIdentificacaoCivilRules"
          label="Nº Identificação Civil"
          required
          v-if="this.$auth.user.groups == 'Atleta' || this.$auth.user.groups == 'Socio'"
        ></v-text-field>
        <v-text-field
          v-model="user.numContribuinte"
          :counter="9"
          :rules="numIdentificacaoCivilRules"
          label="Nº Identificação Civil"
          required
          v-if="this.$auth.user.groups == 'Atleta' || this.$auth.user.groups == 'Socio'"
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
    </div>
  </div>
</template>

<script>
    export default {
        name: "editar",
      data: () => ({
        valid:true,
        user:'',
        old_password:'',
        showPicker: false,
        selectedDate: '',

      }),
      methods: {
        formatDate (date) {
          return moment(date).format('DD-MM-YYYY')
        },
        getUser(){
          if(this.$auth.user.groups == 'Treinador'){
            this.$axios.$get('/api/treinadores/'+this.$route.params.email+'/').then((socio) => {
              this.user = socio;
              this.old_password = socio.password;
            });
          }
          if(this.$auth.user.groups == 'Atleta'){
            this.$axios.$get('/api/atletas/'+this.$route.params.email+'/').then((socio) => {
              this.user = socio;
              this.old_password = socio.password;
            });
          }
          if(this.$auth.user.groups == 'Socio'){
            this.$axios.$get('/api/socios/'+this.$route.params.email+'/').then((socio) => {
              this.user = socio;
              this.old_password = socio.password;
            });
          }
        },
        validate () {
          if (this.$refs.form.validate()) {
            if(this.$auth.user.groups == 'Treinador'){
              this.$axios.$put('/api/treinadores/'+this.$route.params.email, {
                numeroSocio: this.user.numeroSocio,
                nome: this.user.nome,
                email: this.user.email,
                dataNascimento: this.user.dataNascimento,
                numIdentificacaoCivil: this.user.numIdentificacaoCivil,
                numContribuinte: this.user.numContribuinte,
              })
                .then(() => {
                  this.$router.push('/perfil/info');
                })
                .catch(error => {
                  console.log(error)
                })
            }
            if(this.$auth.user.groups == 'Atleta'){
              this.$axios.$put('/api/atletas/'+this.$route.params.email, {
                numeroSocio: this.user.numeroSocio,
                nome: this.user.nome,
                email: this.user.email,
                dataNascimento: this.user.dataNascimento,
                numIdentificacaoCivil: this.user.numIdentificacaoCivil,
                numContribuinte: this.user.numContribuinte,
              })
                .then(() => {
                  this.$router.push('/perfil/info');
                })
                .catch(error => {
                  console.log(error)
                })
            }
            if(this.$auth.user.groups == 'Socio'){
              this.$axios.$put('/api/socios/'+this.$route.params.email, {
                numeroSocio: this.user.numeroSocio,
                nome: this.user.nome,
                email: this.user.email,
                dataNascimento: this.user.dataNascimento,
                numIdentificacaoCivil: this.user.numIdentificacaoCivil,
                numContribuinte: this.user.numContribuinte,
              })
                .then(() => {
                  this.$router.push('/perfil/info');
                })
                .catch(error => {
                  console.log(error)
                })
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
          this.$router.push('/adminer/socios/'+this.$router.params.email+"/info")
        }
      },
      created() {
        this.getUser();
      }
    }
</script>

<style scoped>

</style>
