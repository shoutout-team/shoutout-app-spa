<template>
  <v-container>
    <v-form
      v-model="valid"
      name="register-form"
      class="validate register"
      novalidate
      @submit.prevent="signUp"
    >
      <v-row justify="center">
        <v-col cols="12" xl="10">
          <h2 class="title font-weight-bold">
            Infos zum Inhaber
          </h2>
        </v-col>
      </v-row>
      <v-row justify="center" wrap>
        <v-col cols="12" sm="6" xl="5">
          <v-text-field
            v-model="user.firstname"
            type="text"
            outlined
            hide-details="auto"
            tile
            color="#000"
            label="Vorname des Inhabers"
            :rules="nameRules"
            :validate-on-blur="true"
            class="required"
          />
        </v-col>
        <v-col cols="12" sm="6" xl="5">
          <v-text-field
            v-model="user.lastname"
            type="text"
            hide-details="auto"
            outlined
            tile
            color="#000"
            label="Nachname des Inhabers"
            :rules="nameRules"
            :validate-on-blur="true"
            class="required"
          />
        </v-col>
      </v-row>
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
            minlength="8"
            :validate-on-blur="true"
            :rules="passwordRules"
            tile
            color="#000"
            label="Password"
            class="required"
            @click:append="show = !show"
          />
        </v-col>
      </v-row>
      <v-row justify="center">
        <v-col cols="12" xl="10">
          <v-row d-flex class="align-baseline">
            <v-badge
              avatar
              overlap
              bottom
              offset-x="25"
              offset-y="25"
              class="mr-7 ml-3"
            >
              <template v-slot:badge class="register__profile-avatar">
                <v-avatar size="60">
                  <v-img :src="Icon" />
                </v-avatar>
              </template>

              <v-avatar size="70">
                <v-img :src="avatarPicture" class="register__img">
                  <input type="file" class="register__drop-input" @input="change($event, 'uploadAvatarPicture', 'user')">
                  <v-progress-circular v-if="imageLoading" class="register__img-loader" indeterminate size="64" />
                </v-img>
              </v-avatar>
            </v-badge>
            <p class="body-2 font-weight-bold">
              Lade ein Portrait<br> von dir hoch
            </p>
          </v-row>
        </v-col>
      </v-row>
      <v-row justify="center" class="mt-6">
        <v-col cols="12" xl="10">
          <v-btn
            color="black"
            :depressed="!valid"
            height="44px"
            tile
            dark
            width="100%"
            max-width="426"
            type="submit"
          >
            Jetzt registrieren
          </v-btn>
          <p
            v-for="(err, i) in failureMessages"
            :key="i"
            class="failure"
          >
            {{ i }}: {{ err[0] }}
          </p>
        </v-col>
      </v-row>
    </v-form>
  </v-container>
</template>

<script>
import imageControllerMixin from '@/mixins/imageController.js'
import endpoints from '@/store/utils/endpoints.js'
import Image from '~/assets/shoutout-icon-upload.svg'
import validationMixin from '@/mixins/validations.js'

export default {
  mixins: [imageControllerMixin, validationMixin],
  data () {
    return {
      Icon: Image,
      valid: false,
      imageLoading: false,
      failure: false,
      fallbackAvatarPicture: require('~/assets/shoutout-user-profilbild.png'),
      uploadAvatarPicture: '',
      user: {
        status: '',
        firstname: '',
        lastname: '',
        email: '',
        password: '',
        avatar_key: ''
      },
      failureMessages: {},
      show: false
    }
  },
  computed: {
    formattedUser () {
      return {
        user: {
          name: `${this.user.firstname} ${this.user.lastname}`,
          email: this.user.email,
          password: this.user.password,
          avatar_key: this.user.avatar_key
        }
      }
    },
    avatarPicture () {
      return this.uploadAvatarPicture || this.$store.state.user.avatar_url || this.fallbackAvatarPicture
    }
  },
  methods: {
    async signUp () {
      try {
        this.failure = false
        await this.$axios.$post(endpoints.SIGNUP_ENDPOINT, this.formattedUser)
        this.errorMessage = ''
        this.$emit('success')
      } catch (err) {
        this.failure = true
        this.failureMessages = err.response.data
      }
    }
  }
}
</script>
<style lang="scss">
.register {
  &__drop-input {
    opacity: 0;
    width: 100%;
    height: 100%;
    cursor: pointer;
  }

  &__img {
    cursor: pointer;
    position: relative;
  }

  &__img-loader {
    position: absolute;
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%);
  }

  .v-badge__badge {
    pointer-events: none;
    cursor: pointer;
  }

  .failure {
    margin-top: 20px;
    &::first-letter {
      text-transform: uppercase;
    }
  }
}
</style>
