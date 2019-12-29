<template>
  <v-container
    fluid
    fill-height
  >
    <v-layout
      align-center
      justify-center
    >
      <v-flex
        xs12
        sm8
        md4
      >
        <v-card class="elevation-12">
          <v-toolbar
            color="primary"
            dark
            flat
          >
            <v-toolbar-title>Login form</v-toolbar-title>
            <v-spacer></v-spacer>


          </v-toolbar>
          <v-card-text>
            <v-form @submit.prevent="onSubmit">
              <v-text-field
                label="Email"
                prepend-icon='mdi-account-plus'
                type="email"s
                v-model="email"
              ></v-text-field>
              {{this.email}}

              <v-text-field
                id="password"
                label="Password"

                prepend-icon='mdi-lock'
                type="password"
                v-model="password"
              ></v-text-field>
              {{this.password}}
              <v-btn type="submit">Submit</v-btn>
            </v-form>
          </v-card-text>
          <v-card-actions>
            <v-spacer></v-spacer>
            <v-btn color="primary">Login</v-btn>
          </v-card-actions>
        </v-card>
      </v-flex>
    </v-layout>
  </v-container>
</template>

<script>
  export default {
    auth: false,
    data() {
      return {
        email: null,
        password: null,
      };
    },
    methods: {
      onSubmit() {
        let promise = this.$auth.loginWith('local', {
          data: {
            email: this.email,
            password: this.password
          }
        });
        promise.then(() => {
          this.$toast.success('You are logged in!')
          // check if the user $auth.user object is set
          console.log(this.$auth.user)
          // TODO redirect based on the user role
          // eg:
          // if (this.$auth.user.groups.includes('Teacher')) {
          // this.$router.push('url-to-teacher-subjects')
          // } else if (...) {
          // ...
          // }
        });
        promise.catch(() => {
          this.$toast.error('Sorry, you cant login. Ensure your credentials are correct')
        })
      },
      onReset() {
        this.email = null;
        this.password = null;
      }
    }
  }
</script>

<style scoped>

</style>
