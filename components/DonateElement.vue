<template>
  <a
    v-bind="link"
    target="_blank"
    @click="toggleIBAN"
  >
    <v-img v-if="!showIBAN" class="button" width="100%" :alt="payment.type" :src="images[payment.type]" />
    <p v-if="payment.type === 'bank' && showIBAN" class="title">Name: {{ payment.paymentInfo.owner }}</p>
    <p v-if="payment.type === 'bank' && showIBAN" class="title">IBAN: {{ payment.paymentInfo.iban }}</p>
  </a>
</template>

<script>
import { mdiArrowRightThick } from '@mdi/js'

export default {
  props: {
    payment: {
      type: Object,
      default: () => {}
    }
  },
  data () {
    return {
      mdiArrowRightThick,
      images: {
        paypal: require('~/assets/icon-paypal.png'),
        gofoundme: require('~/assets/icon-gofundme.png'),
        bank: require('~/assets/icon-transaction.svg'),
        ticketio: require('~/assets/icon-ticketio.png'),
        startnext: require('~/assets/icon-startnext.png')
      },
      showIBAN: false
    }
  },
  computed: {
    link () {
      return this.payment.type !== 'bank' ? { href: this.validatedLink(this.payment.paymentInfo) } : { disabled: true }
    }
  },
  methods: {
    toggleIBAN () {
      if (this.payment.type === 'bank') {
        this.showIBAN = !this.showIBAN
      }
    },
    validatedLink (url) {
      if (url.startsWith('http')) { return url }
      return `https://${url}`
    }
  }
}
</script>

<style scoped lang="scss">
.button {
  cursor: pointer;

  &:hover {
    transform: translate(0%, -25%) scale(1.2);
  }
}
</style>
