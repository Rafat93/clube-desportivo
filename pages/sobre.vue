<template>
  <div>
    <v-row>
      <v-col>
        <v-card>
          <v-card-title class="justify-center">
            <div>Sobre o </div>
          </v-card-title>
          <v-divider></v-divider>
        </v-card>
      </v-col>
    </v-row>
  </div>
</template>

<script>
    export default {
        name: "sobre",
      data: () => ({
          clube: '',
      }),
      methods:{
          getClube(){
            this.$axios.$get('/api/clube/').then((clube) => {
              this.clube = clube;
            });
          }
      },
      created() {
          this.getClube();
      }

    }
</script>

<style scoped>

</style>
