<template>
  <v-app light>
    <v-navigation-drawer
      v-model="drawer"
      :mini-variant="miniVariant"
      :clipped="clipped"
      fixed
      app
    >
      <v-list v-if="this.$auth.loggedIn">
        <div v-if="this.$auth.user.groups == 'Administrador'">
          <v-list-item
            v-for="(item, i) in itemsAdmin"
            :key="i"
            :to="item.to"
            router
            exact
          >
            <v-list-item-action>
              <v-icon>{{ item.icon }}</v-icon>
            </v-list-item-action>
            <v-list-item-content>
              <v-list-item-title v-text="item.title" />
            </v-list-item-content>
          </v-list-item>
          <v-list-item @click.prevent="logout" >
            <v-list-item-action>
              <v-icon>{{this.itemLogout}}</v-icon>
            </v-list-item-action>
            <v-list-item-title>Logout</v-list-item-title>
          </v-list-item>
        </div>


        <!--  Se não for ADMIN-->
        <div v-if="this.$auth.user.groups == 'Atleta'">
          <v-list-item
            v-for="(item, i) in items_atleta"
            :key="i"
            :to="item.to"
            router
            exact
          >
            <v-list-item-action>
              <v-icon>{{ item.icon }}</v-icon>
            </v-list-item-action>
            <v-list-item-content>
              <v-list-item-title v-text="item.title" />
            </v-list-item-content>
          </v-list-item>
          <v-list-item @click.prevent="logout">
            <v-list-item-action>
              <v-icon>{{this.itemLogout}}</v-icon>
            </v-list-item-action>
            <v-list-item-title>Logout</v-list-item-title>
          </v-list-item>
        </div>

        <div v-if="this.$auth.user.groups == 'Treinador'">
          <v-list-item
            v-for="(item, i) in items_treinador"
            :key="i"
            :to="item.to"
            router
            exact
          >
            <v-list-item-action>
              <v-icon>{{ item.icon }}</v-icon>
            </v-list-item-action>
            <v-list-item-content>
              <v-list-item-title v-text="item.title" />
            </v-list-item-content>
          </v-list-item>
          <v-list-item @click.prevent="logout">
            <v-list-item-action>
              <v-icon>{{this.itemLogout}}</v-icon>
            </v-list-item-action>
            <v-list-item-title>Logout</v-list-item-title>
          </v-list-item>
        </div>

        <div v-if="this.$auth.user.groups == 'Socio'">
          <v-list-item
            v-for="(item, i) in items_socio"
            :key="i"
            :to="item.to"
            router
            exact
          >
            <v-list-item-action>
              <v-icon>{{ item.icon }}</v-icon>
            </v-list-item-action>
            <v-list-item-content>
              <v-list-item-title v-text="item.title" />
            </v-list-item-content>
          </v-list-item>
          <v-list-item @click.prevent="logout">
            <v-list-item-action>
              <v-icon>{{this.itemLogout}}</v-icon>
            </v-list-item-action>
            <v-list-item-title>Logout</v-list-item-title>
          </v-list-item>
        </div>
      </v-list>



      <v-list v-if="!this.$auth.loggedIn">
        <v-list-item
          v-for="(button, i) in buttons"
          :key="i"
          :to="button.to"
          router
          exact
        >
          <v-list-item-action>
            <v-icon>{{ button.icon }}</v-icon>
          </v-list-item-action>
          <v-list-item-content>
            <v-list-item-title v-text="button.title" />
          </v-list-item-content>
        </v-list-item>
      </v-list>
    </v-navigation-drawer>
    <v-app-bar
      :clipped-left="clipped"
      fixed
      app
    >
      <v-app-bar-nav-icon @click.stop="drawer = !drawer" />
      <v-btn
        icon
        @click.stop="miniVariant = !miniVariant"
      >
        <v-icon>mdi-{{ `chevron-${miniVariant ? 'right' : 'left'}` }}</v-icon>
      </v-btn>
      <v-btn
        icon
        @click.stop="clipped = !clipped"
      >
        <v-icon>mdi-application</v-icon>
      </v-btn>
      <v-btn
        icon
        @click.stop="fixed = !fixed"
      >
        <v-icon>mdi-minus</v-icon>
      </v-btn>
      <v-toolbar-title align="center" v-text="title" />
      <v-spacer />

    </v-app-bar>
    <v-content>
      <v-container>
        <nuxt />

      </v-container>
    </v-content>

    <v-footer
      :fixed="fixed"
      app
    >
      <!--<span>&copy; 2019</span>-->
    </v-footer>
  </v-app>
</template>

<script>
export default {
  data () {

    return {
      clipped: false,
      drawer: false,
      fixed: false,
      itemLogout: 'mdi-power',

      buttons:[
        {
          icon: 'mdi-apps',
          title: 'Welcome',
          to: '/'
        },
        {
          icon: 'mdi-account-multiple',
          title: 'Login',
          to: '/auth/login'
        },{
          icon: 'mdi-account-plus',
          title: 'Register',
          to: '/register'
        },
        {
          icon: 'mdi-information-outline',
          title: 'Sobre',
          to: '/sobre'
        },
      ],

      itemsAdmin: [
        {
          icon: 'mdi-apps',
          title: 'Welcome',
          to: '/'
        },
        {
          icon: 'mdi-football',
          title: 'Modalidades',
          to: '/adminer/modalidades/list'
        },
        {
          icon: 'mdi-run',
          title: 'Atletas',
          to: '/adminer/atletas/list'
        },
        {
          icon: 'mdi-diamond-stone',
          title: 'Produtos',
          to: '/adminer/produtos/list'
        },
        {
          icon: 'mdi-check-box-outline',
          title: 'Inscrições',
          to: '/adminer/inscricoes/list'
        },
        {
          icon: 'mdi-account-group',
          title: 'Sócios',
          to: '/adminer/socios/list'
        },
        {
          icon: 'mdi-account-star',
          title: 'Treinadores',
          to: '/adminer/treinadores/list'
        },
        {
          icon: 'mdi-information-outline',
          title: 'Sobre',
          to: '/sobre'
        },
      ],
      items_atleta: [
        {
          icon: 'mdi-apps',
          title: 'Welcome',
          to: '/'
        },
        {
          icon: 'mdi-account',
          title: 'Perfil',
          to: '/perfil/info'
        },
        {
          icon: 'mdi-basketball',
          title: 'Modalidades',
          to: '/modalidades/list'
        },
        {
          icon: 'mdi-basketball',
          title: 'Produtos',
          to: '/produtos/list'
        },
        {
          icon: 'mdi-information-outline',
          title: 'Sobre',
          to: '/sobre'
        },
      ],
      items_socio: [
        {
          icon: 'mdi-apps',
          title: 'Welcome',
          to: '/'
        },
        {
          icon: 'mdi-account',
          title: 'Perfil',
          to: '/perfil/info'
        },
        {
          icon: 'mdi-basketball',
          title: 'Modalidades',
          to: '/modalidades/list'
        },
        {
          icon: 'mdi-basketball',
          title: 'Produtos',
          to: '/produtos/list'
        },
        {
          icon: 'mdi-information-outline',
          title: 'Sobre',
          to: '/sobre'
        },
      ],
      items_treinador: [
        {
          icon: 'mdi-apps',
          title: 'Welcome',
          to: '/'
        },
        {
          icon: 'mdi-account',
          title: 'Perfil',
          to: '/perfil/info'
        },
        {
          icon: 'mdi-basketball',
          title: 'Modalidades',
          to: '/modalidades/list'
        },{
          icon: 'mdi-information-outline',
          title: 'Sobre',
          to: '/sobre'
        },
      ],
      miniVariant: false,
      right: true,
      rightDrawer: false,
      title: 'Clube Desportivo'
    }

  },
  methods:{
    logout () {
      this.$auth.logout('local')
    }
  }
}
</script>
