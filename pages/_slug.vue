<template>
  <v-row>
    <v-spacer />
    <envelop v-if="data" :message="data.message" :image="data.image" :confetti="data.confetti" />
    <v-spacer />
  </v-row>
</template>

<script>
export default {
  fetch () {
    try {
      const json = Buffer.from(this.$route.params.slug, 'base64').toString()
      this.data = JSON.parse(json)
    } catch (e) {
      this.$nuxt.error({ statusCode: 404, message: 'Page not found' })
    }
  },
  data: () => {
    return {
      data: null
    }
  }
}
</script>
