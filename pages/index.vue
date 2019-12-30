<template>
  <div>
    <v-data-table
      :headers="headers"
      :items="atletas"
      :items-per-page="10"
      class="elevation-1"
    >
      <template slot="items" slot-scope="props">
        <tr @click="props.expanded = !props.expanded">
          <td class="text-xs">{{ props.item.nome }}</td>
          <td class="text-xs">{{ props.item.email}}</td>
        </tr>
      </template>
    </v-data-table>
  </div>

</template>

<script>
export default {
  data () {
    return {
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
        }
      ],
      atletas: []
    }
  },
  created () {

    this.$axios.$get('/api/atletas')
      .then((atletas) => {
        this.atletas = atletas
      });


  }
}
</script>
