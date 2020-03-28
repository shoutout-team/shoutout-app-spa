<template>
  <div>
    <v-dialog :value="!Object.keys(activeUser).length" fullscreen hide-overlay transition="dialog-bottom-transition">
      <Modal :title="fallBackModal.title" :message="fallBackModal.message" :link="fallBackModal.link.title" @link="toLogin" />
    </v-dialog>
    <v-dialog :value="request === 'success'" fullscreen hide-overlay transition="dialog-bottom-transition">
      <Modal :title="successModal.title" :message="successModal.message" :link="successModal.link.title" @link="toOverview" />
    </v-dialog>
    <v-container v-if="Object.keys(activeUser).length" class="edit-company">
      <v-row justify="center">
        <v-col>
          <v-form
            name="register-form"
            class="validate"
            target="_blank"
            novalidate
            @submit.prevent="updateInfo"
          >
            <v-row justify="center">
              <v-col cols="12" xl="10">
                <v-img alt="shoutout tape" max-width="300px" :src="require('~/assets/shoutout-tape.png')">
                  <h1 class="edit-company__title font-weight-black pt-3">
                    Dein Profil
                  </h1>
                </v-img>
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
                  v-model="company.name"
                  type="text"
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
                  v-model="company.properties.crnumber"
                  type="text"
                  hide-details="auto"
                  outlined
                  tile
                  color="#000"
                  label="Handsregisternummer"
                  class="required"
                />
              </v-col>
            </v-row>
            <v-row justify="center">
              <v-col cols="12" sm="6" xl="5">
                <address-autocomplete @location="getAddressData" />
              </v-col>
              <v-col cols="12" sm="6" xl="5">
                <v-select
                  v-model="company.category"
                  class="edit-company__select"
                  :items="categoryKeys"
                  color="black"
                  label="Kategorie"
                  no-data-text="Kategorie"
                  outlined
                />
              </v-col>
            </v-row>
            <v-row justify="center">
              <v-col cols="12" xl="10">
                <h2 class="title font-weight-bold mt-7">
                  Spenden Optionen
                </h2>
              </v-col>
            </v-row>
            <v-row justify="center">
              <v-col cols="12" sm="4">
                <v-text-field
                  v-model="company.properties.payment.paypal"
                  type="text"
                  hide-details="auto"
                  outlined
                  tile
                  color="#000"
                  label="Paypal Link"
                  class="required"
                />
              </v-col>
              <v-col cols="12" sm="4">
                <v-text-field
                  v-model="company.properties.payment.gofoundme"
                  type="text"
                  hide-details="auto"
                  outlined
                  tile
                  color="#000"
                  label="GoFundMe Link"
                  class="required"
                />
              </v-col>
              <v-col cols="12" sm="4">
                <v-text-field
                  v-model="company.properties.payment.bank.iban"
                  type="text"
                  hide-details="auto"
                  outlined
                  tile
                  color="#000"
                  label="IBAN"
                  class="required"
                />
              </v-col>
            </v-row>
            <v-row justify="center">
              <v-col cols="12" xl="10">
                <h2 class="title font-weight-bold mt-10">
                  Social Media
                </h2>
              </v-col>
            </v-row>
            <v-row justify="center">
              <v-col cols="12" sm="4">
                <v-text-field
                  v-model="company.properties.links.facebook"
                  type="text"
                  hide-details="auto"
                  outlined
                  tile
                  color="#000"
                  label="Facebook"
                  class="required"
                />
              </v-col>
              <v-col cols="12" sm="4">
                <v-text-field
                  v-model="company.properties.links.instagram"
                  type="text"
                  hide-details="auto"
                  outlined
                  tile
                  color="#000"
                  label="Instagram"
                  class="required"
                />
              </v-col>
              <v-col cols="12" sm="4">
                <v-text-field
                  v-model="company.properties.links.website"
                  type="text"
                  hide-details="auto"
                  outlined
                  tile
                  color="#000"
                  label="Website"
                  class="required"
                />
              </v-col>
            </v-row>
            <v-row justify="center" class="align-center">
              <v-col cols="12" sm="6" xl="5">
                <p class="caption font-weight-bold">
                  Bild von deinem Unternehmen
                </p>
                <v-img class="mx-auto edit-company__upload" max-width="600px" width="100%" :src="companyPicture">
                  <input ref="fileInput" type="file" class="drop__input" @input="change($event, 'uploadCompanyPicture', 'company')">
                  <v-avatar color="black" class="edit-company__upload-icon">
                    <v-img :src="Icon" />
                  </v-avatar>
                  <v-progress-circular v-if="imageLoading" class="edit-company__upload-loader" indeterminate size="64" />
                </v-img>
                <!-- <v-row class="align-center">
                  <v-col v-for="(image, key) in images" :key="key" cols="3">
                    <v-img height="60px" :alt="image.alt" :src="image.src" @click="dropImage(key)" />
                  </v-col>
                </v-row> -->
              </v-col>
              <v-col cols="12" sm="6" xl="5" class="mt-12">
                <h2 class="title font-weight-bold">
                  Persönliches Anschreiben
                </h2>
                <v-textarea
                  v-model="company.properties.notes"
                  auto-grow
                  outlined
                  rows="7"
                  label="Deine Worte an die Community"
                  class="mt-6"
                  hint="Max. 500 Zeichen"
                />
              </v-col>
            </v-row>
            <v-row>
              <v-col cols="12">
                {{ failure }}
              </v-col>
            </v-row>
            <v-row class="align-baseline" justify="center">
              <v-col cols="12" sm="6" xl="5">
                <v-btn
                  nuxt
                  to="/"
                  depressed
                  width="100%"
                >
                  <v-avatar color="black" size="70" class="mr-5">
                    <v-icon size="40" color="white">
                      {{ mdiEye }}
                    </v-icon>
                  </v-avatar>
                  <p class="body-2 font-weight-bold">
                    Vorschau deiner Unternehmensseite
                  </p>
                </v-btn>
              </v-col>
              <v-col cols="12" sm="6" xl="5" class="my-12">
                <v-btn
                  color="black"
                  depressed
                  dark
                  width="100%"
                  height="50px"
                  tile
                  type="submit"
                >
                  Speichern
                </v-btn>
              </v-col>
            </v-row>
          </v-form>
        </v-col>
      </v-row>
    </v-container>
  </div>
</template>

<script>
import { mdiEye } from '@mdi/js'
import Modal from '~/components/Modal.vue'
import imageControllerMixin from '@/mixins/imageController.js'
import Image from '~/assets/shoutout-icon-upload.svg'
import AddressAutocomplete from '@/components/AddressAutocomplete.vue'

export default {
  components: {
    AddressAutocomplete,
    Modal
  },
  mixins: [imageControllerMixin],
  data () {
    return {
      Icon: Image,
      mdiEye,
      imageLoading: false,
      failure: '',
      uploadCompanyPicture: '',
      fallbackCompanyPicture: require('~/assets/shoutout-profilbild-platzhalter.jpg'),
      user: {
        firstname: '',
        lastname: '',
        email: ''
      },
      company: {
        name: '',
        title: '',
        category: '',
        postcode: '',
        city: '',
        street: '',
        street_number: '',
        latitude: '',
        longitude: '',
        keeper_token: '',
        properties: {
          description: '',
          crnumber: '',
          notes: '',
          payment: {
            paypal: '',
            gofoundme: '',
            bank: {
              owner: '',
              iban: ''
            }
          },
          links: {
            website: '',
            facebook: '',
            twitter: '',
            instagram: ''
          }
        }
      },
      fallBackModal: {
        title: 'Sorry',
        message: 'Bitte melde dich an um diese Seite zu sehen',
        link: { title: 'Zum Login', url: '/signup' }
      },
      successModal: {
        title: 'Vielen Dank',
        message: 'Die Anmeldung für dein Unternehmen ist erfolgreich eingegangen. Wir überprüfen deine Angaben und schalten dein Eintrag anschließend frei.',
        link: { title: 'Zur Übersicht', url: '/overview' }
      }
    }
  },
  computed: {
    companyPicture () {
      return this.uploadCompanyPicture || this.activeCompany.picture_url || this.fallbackCompanyPicture
    },
    categoryKeys () {
      return Object.keys(this.$store.state.categories)
    },
    activeCompany () {
      return this.$store.state.companies.find(c => c.gid === this.$store.state.user.gid) || {}
    },
    activeUser () {
      return this.$store.state.user
    },
    request () {
      return this.$store.state.company_request
    }
  },
  mounted () {
    if (Object.keys(this.activeCompany).length) {
      this.company = this.activeCompany
    }
  },
  methods: {
    getAddressData (place) {
      this.company.latitude = place.latitude
      this.company.longitude = place.longitude
      this.company.city = place.locality
      this.company.street = place.route
      this.company.postcode = place.postal_code
      this.company.street_number = place.street_number
    },
    updateInfo () {
      this.$store.dispatch('postCompany', {
        ...this.company,
        title: this.company.name,
        keeper_token: this.$store.state.user.gid
      })
    },
    toLogin () {
      this.$router.push({ path: this.fallBackModal.link.url })
    },
    toOverview () {
      this.$router.push({ path: this.successModal.link.url })
    }
  }
}
</script>

<style lang="scss">
.v-list-item__content {
  text-transform: capitalize;
}

.drop__input {
  cursor: pointer;
}
.edit-company {
  &__select {
    text-transform: capitalize;
  }

  &__upload {
    position: relative;
    cursor: pointer;

    &:hover {
      .edit-company__upload-icon {
        transform: translate(-50%, -50%) scale(1.1);
      }
    }
  }

  .edit-company {
    &__title {
      font-family: 'theRambler';
      position: absolute;
      font-size: 3.3rem;
    }
  }

  .v-badge__badge {
    pointer-events: none;
    cursor: pointer;
  }

  &__upload-icon {
    left: 50%;
    top: 50%;
    position: absolute !important;
    transform: translate(-50%, -50%);
    transition: transform 200ms ease;
    pointer-events: none;
  }

  &__upload-loader {
    top: 50%;
    left: 50%;
    position: absolute;
    transform: translate(-50%, -50%);
  }

  .drop {
    &__input {
      opacity: 0;
      width: 100%;
      height: 100%;
    }
  }
}
</style>