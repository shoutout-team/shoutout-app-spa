<template>
  <v-container class="signup">
    <v-row justify="center">
      <v-col>
        <v-dialog v-model="modal" fullscreen hide-overlay transition="dialog-bottom-transition">
          <Modal title="Danke!" message="Deine Registrierung war erfolgreich!" link="Zum Login" @link="closeModal" />
        </v-dialog>
        <v-row justify="center">
          <v-col cols="12" xl="10">
            <v-img alt="shoutout tape" max-width="300px" :src="require('~/assets/shoutout-tape.png')">
              <h1 class="signup__title pt-3">
                {{ headline }}
              </h1>
            </v-img>
          </v-col>
        </v-row>
        <v-row v-if="status === 'new'" justify="center" wrap>
          <v-col cols="12" sm="6" xl="5">
            <v-radio-group v-model="status" :mandatory="true" row wrap>
              <v-radio color="black" label="Ich bin neu hier" value="new" class="mt-4" />
              <v-radio color="black" label="Ich bin bereits registriert" value="recurring" class="mt-4" />
            </v-radio-group>
          </v-col>
          <v-col cols="12" sm="6" xl="5">
            <p>
              Damit dich Supporter finden können, brauchen wir ein paar Infos von dir.
              Nach der Registrierung schalten wir dich frei und du kannst dein Profil anlegen.
            </p>
          </v-col>
        </v-row>
        <v-row v-else justify="center" wrap>
          <v-col cols="12" xl="10">
            <v-radio-group v-model="status" :mandatory="true" row wrap>
              <v-radio color="black" label="Ich bin neu hier" value="new" class="mt-4" />
              <v-radio color="black" label="Ich bin bereits registriert" value="recurring" class="mt-4" />
            </v-radio-group>
          </v-col>
        </v-row>
        <register-form v-if="status === 'new'" @success="showModal" />
        <login-form v-if="status === 'recurring'" />
      </v-col>
    </v-row>
  </v-container>
</template>

<script>
import Modal from '@/components/Modal.vue'
import RegisterForm from '@/components/RegisterForm.vue'
import LoginForm from '@/components/LoginForm.vue'

export default {
  components: {
    Modal,
    RegisterForm,
    LoginForm
  },
  data () {
    return {
      status: 'new',
      modal: false
    }
  },
  computed: {
    headline () {
      return this.status === 'new' ? 'Registrieren' : 'Melde dich an'
    }
  },
  beforeMount () {
    if (this.$store.state.user.gid) {
      this.$router.replace('/edit-company')
    }

    if (this.$route.params.status === 'login') {
      this.status = 'recurring'
    }
  },
  methods: {
    showModal () {
      window.scrollTo(0, 0)
      this.modal = true
    },
    closeModal () {
      this.status = 'recurring'
      this.modal = false
    }
  }
}
</script>

<style lang="scss" scoped>
.signup {
  &__title {
    font-family: 'theRambler';
    font-weight: 500;
    font-size: 4rem;
    padding-left: 26px;
    margin-top: -10px;
  }
}
</style>
