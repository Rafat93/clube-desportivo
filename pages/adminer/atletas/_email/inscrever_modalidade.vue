<template>
  <v-form ref="form" v-model="valid" lazy-validation>
    <p class="subtitle-1 text-center">Inscrição em Nova Modalidade</p>
    <v-select
      v-model="treinadorSelecionado"
      :items="all_modalidades"
      label="Selecione"
      item-text="nome"
      item-value="email"
      chips
      hint="Selecione um treinador."
      persistent-hint
    ></v-select>
    <v-text-field
      v-model="idadeMin"
      :counter="2"
      :rules="idadeMinRules"
      type="number"
      label="Idade Minima"
      required
    ></v-text-field>
    <v-text-field
      v-model="idadeMax"
      :counter="2"
      :rules="idadeMaxRules"
      label="Idade Máxima"
      required
      type="number"
    ></v-text-field>

    <v-btn :disabled="!valid" color="success" class="mr-4" @click="validate">
      Adicionar
    </v-btn>

    <v-btn color="error" class="mr-4" @click="reset">
      Reset Formulário
    </v-btn>

    <v-btn color="warning" class="mr-4" @click="resetValidation">
      Reset Validação
    </v-btn>
    <v-btn color="error" @click="cancel">
      Cancelar
    </v-btn>
  </v-form>
</template>

<script>
    export default {
        name: "inscrever_modalidade"
    }
</script>

<style scoped>

</style>
