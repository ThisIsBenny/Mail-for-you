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
    this.data = JSON.parse(Buffer.from(this.$route.params.slug, 'base64').toString())
  },
  data: () => {
    return {
      data: null
    }
  }
}
</script>
