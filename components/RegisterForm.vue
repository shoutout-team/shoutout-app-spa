<template>
  <v-container>
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
          type="text"
          :value="user.firstname"
          outlined
          hide-details="auto"
          tile
          color="#000"
          label="Vorname des Inhabers"
          class="required"
        />
      </v-col>
      <v-col cols="12" sm="6" xl="5">
        <v-text-field
          type="text"
          :value="user.lastname"
          hide-details="auto"
          outlined
          tile
          color="#000"
          label="Nachnahme des Inhabers"
          class="required"
        />
      </v-col>
    </v-row>
    <v-row justify="center">
      <v-col cols="12" sm="6" xl="5">
        <v-text-field
          type="email"
          :value="user.email"
          hide-details="auto"
          outlined
          tile
          color="#000"
          label="E-Mail Adresse"
          class="required"
        />
      </v-col>
      <v-col cols="12" sm="6" xl="5">
        <v-text-field
          :append-icon="show ? 'mdi-eye' : 'mdi-eye-off'"
          :type="show ? 'text' : 'password'"
          :value="user.password"
          hide-details="auto"
          outlined
          tile
          color="#000"
          label="Password"
          class="required"
          @click="show = !show"
        />
      </v-col>
    </v-row>
    <v-row justify="center">
      <v-col cols="12" xl="10">
        <h2 class="title font-weight-bold mt-7">
          Infos zum Unternehmen
        </h2>
      </v-col>
    </v-row>
    <v-row justify="center">
      <v-col cols="12" sm="6" xl="5">
        <v-text-field
          type="text"
          :value="user.company_name"
          hide-details="auto"
          outlined
          tile
          color="#000"
          label="Name des Unternehmens"
          class="required"
        />
      </v-col>
      <v-col cols="12" sm="6" xl="5">
        <v-text-field
          type="text"
          :value="user.hand_registe_number"
          hide-details="auto"
          outlined
          tile
          color="#000"
          label="Handsregisternummer"
          class="required"
        />
      </v-col>
    </v-row>
    <v-row justify="center" class="mt-6">
      <v-col cols="12" xl="10">
        <v-btn
          color="black"
          depressed
          height="44px"
          tile
          dark
          width="100%"
          max-width="426"
          nuxt
          to="/login"
        >
          Jetzt registrieren
        </v-btn>
      </v-col>
    </v-row>
  </v-container>
</template>

<script>
export default {
  data () {
    return {
      user: {
        status: '',
        firstname: '',
        lastname: '',
        email: '',
        password: '',
        company_name: '',
        hand_register_number: ''
      },
      show: false
    }
  }
}
</script>