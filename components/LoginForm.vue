<template>
  <v-container>
    <v-form
      v-model="valid"
      name="register-form"
      class="validate"
      novalidate
      @submit.prevent="login"
    >
      <v-row justify="center">
        <v-col cols="12" sm="6" xl="5">
          <v-text-field
            v-model="user.email"
            type="email"
            hide-details="auto"
            outlined
            tile
            color="#000"
            label="E-Mail Adresse"
            :validate-on-blur="true"
            :rules="emailRules"
            class="required"
          />
        </v-col>
        <v-col cols="12" sm="6" xl="5">
          <v-text-field
            v-model="user.password"
            :append-icon="show ? 'mdi-eye' : 'mdi-eye-off'"
            :type="show ? 'text' : 'password'"
            hide-details="auto"
            outlined
            tile
            color="#000"
            label="Password"
            :validate-on-blur="true"
            :rules="passwordRules"
            @click:append="show = !show"
          />
        </v-col>
      </v-row>
      <v-row justify="center" class="mt-6">
        <v-col cols="12" xl="10">
          <p v-if="loginRequest === 'failed'">
            Es scheint ein Fehler aufgetreten zu sein. Bitte überprüfe deine Zugangsdaten.
          </p>
          <v-btn
            color="black"
            depressed
            height="44px"
            tile
            dark
            width="100%"
            max-width="426"
            :loading="loginRequest === 'pending'"
            type="submit"
          >
            Jetzt anmelden
          </v-btn>
        </v-col>
      </v-row>
    </v-form>
  </v-container>
</template>

<script>
import validationMixin from '@/mixins/validations.js'

export default {
  mixins: [validationMixin],
  data () {
    return {
      valid: false,
      user: {
        email: '',
        password: ''
      },
      show: false
    }
  },
  computed: {
    loginRequest () {
      return this.$store.state.login_request
    }
  },
  watch: {
    loginRequest (value) {
      if (value === 'success') {
        this.$router.push({ path: '/edit-company' })
      }
    }
  },
  methods: {
    login () {
      this.$store.dispatch('login', this.user)
    }
  }
}
</script>
