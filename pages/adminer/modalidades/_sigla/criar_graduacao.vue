<template>
  <v-form
    ref="form"
    v-model="valid"
    lazy-validation
  >
    <p class="subtitle-1 text-center">Criação de Nova Modalidade</p>
    <v-text-field
      v-model="sigla"
      :counter="5"
      :rules="siglaRules"
      label="Sigla"
      required
    ></v-text-field>
    <v-text-field
      v-model="nome"
      :counter="20"
      :rules="nomeRules"
      label="Nome"
      required
    ></v-text-field>
    <v-text-field
      v-model="epoca"
      :counter="9"
      :rules="epocaRules"
      label="Época"
      hint="20XX/20XX"
      required
    ></v-text-field>

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
        name: "criar_graduacao"
    }
</script>

<style scoped>

</style>
