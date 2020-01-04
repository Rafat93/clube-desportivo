<template>
  <div>
    <v-row>
      <v-col cols="4">
        <div style="display: block">
          <v-text-field
            v-model="search"
            label="Pesquisa"
            hide-details
          ></v-text-field>
        </div>
      </v-col>
      <v-col align="center">
        <v-btn small style="margin-bottom: -50px;" @click="redirectCriar"><v-icon>{{'mdi-plus'}}</v-icon>Administrador</v-btn>
      </v-col>
    </v-row>

    <v-data-table
      :headers="headers"
      :items="admins"
      :items-per-page="10"
      class="elevation-1"
      :search="search"
    >
      <template v-slot:item.action="{ item }" >
        <v-icon
          @click="redirectInfo(item)"
        >
          {{'mdi-information-outline'}}
        </v-icon>
      </template>
    </v-data-table>

  </div>
</template>

<script>

    export default {
        name: "list",
      data () {
        return {
          dialog: false,
          search: '',
          headers: [
            {
              text: 'Nome',
              align: 'left',
              sortable: false,
              value: 'nome'
            },
            {
              text: 'Email',
              align: 'left',
              sortable: false,
              value: 'email'
            },
            { text: 'Ações', value: 'action', sortable: false },

          ],
          admins: [],
        }
      },
      methods:{
        redirectInfo(item){
          this.$router.push('/adminer/admins/'+item.email+'/info');
        },
        redirectCriar(){
          this.$router.push('/adminer/admins/criar_administrador');
        },
        getAdmins(){
          this.$axios.$get('/api/administradores')
            .then((admins) => {
              this.admins = admins;
            });
        }

      },
      created () {
        this.getAdmins();
      }
    }
</script>

<style scoped>

</style>
