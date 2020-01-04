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

    </v-row>
    <v-data-table
      :headers="headers"
      :items="inscricoes"
      :search="search"
      :items-per-page="10"
      class="elevation-1"
    >
      <template v-slot:item.action="{ item }">
        <v-icon

          class="mr-2"
          @click="acceptItem(item)"
        >
          {{'mdi-check-box-outline'}}
        </v-icon>
        <v-icon @click="deleteItem(item)">
          {{'mdi-delete'}}
        </v-icon>
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
          search: '',
          color: '',
          mode: '',
          snackbar: false,
          text: '',
          timeout: 4000,
          x: null,
          y: 'top',
          headers: [
            {
              text: 'Codigo',
              align: 'left',
              sortable: false,
              value: 'code'
            },
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
            {
              text: 'Data Nascimento',
              align: 'left',
              sortable: false,
              value: 'dataNascimento'
            },
            {
              text: 'Nº Identificação Civil',
              align: 'left',
              sortable: false,
              value: 'numIdentificacaoCivil'
            },
            {
              text: 'Nº Identificação Fiscal',
              align: 'left',
              sortable: false,
              value: 'numContribuinte'
            },
            {
              text: 'Morada',
              align: 'left',
              sortable: false,
              value: 'morada'
            },
            { text: 'Actions', value: 'action', sortable: false },

          ],
          inscricoes: []
        }
      },
      created () {

          this.getInscricoes();

      },
      methods:{

        getInscricoes (){
          this.$axios.$get('/api/inscricoes/pendentes')
            .then((inscricoes) => {
              this.inscricoes = inscricoes;
            });
        },
        redirectInfo(item){
          this.$router.push('/adminer/inscricoes/'+item.code+'/info');
        },
        acceptItem (item) {
          this.$axios.$put('api/inscricoes/'+item.code+'/confirm').then( inscricoes => {
            this.getInscricoes(),

              // show snackbar
            this.color = 'green',
            this.text = 'Aceite com sucesso!',
            this.snackbar = true
            }
          ).catch(error => {

              this.color = 'red',
              this.text = 'ERRO: Sócio com o email '+item.email+ ' já existe.',
              this.snackbar = true
          });
        },
        deleteItem (item) {
          let response = confirm('Tem a certeza que deseja remover a inscrição?');
          if(response == true){
              this.$axios.$delete('/api/inscricoes/'+item.code).then( inscricoes =>
                {
                  this.color = 'green';
                  this.text = 'Inscrição com o código - '+item.code+' - eliminada com sucesso!';
                  this.snackbar = true;
                  this.getInscricoes();
                }
              ).catch(error =>{
                console.log(error)
              });
          }
        },
      }
    }
</script>

<style scoped>

</style>
