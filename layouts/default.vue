<template>
  <v-app>
    <v-main>
      <v-container>
        <nuxt />
      </v-container>
    </v-main>
  </v-app>
</template>

<script>
export default {
  head () {
    const i18nSeo = this.$nuxtI18nSeo()
    return {
      title: this.$t('title'),
      htmlAttrs: {
        ...i18nSeo.htmlAttrs
      },
      meta: [
        ...i18nSeo.meta
      ],
      link: [
        ...i18nSeo.link
      ]
    }
  }
}
</script>

<style scoped>
.v-application {
  background-color: #99c;
}
.theme--light.v-application {
  background-color: #99c;
}
</style>
