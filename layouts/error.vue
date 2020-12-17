<template>
  <v-row>
    <v-col align="center" cols="12">
      <h1 v-if="error.statusCode === 404" class="display-4">
        {{ pageNotFound }}
      </h1>
      <h1 v-else class="display-4">
        {{ otherError }}
      </h1>
    </v-col>
  </v-row>
</template>

<script>
export default {
  props: {
    error: {
      type: Object,
      default: null
    }
  },
  data () {
    return {
      pageNotFound: '404 Not Found',
      otherError: 'An error occurred'
    }
  },
  head () {
    const title =
      this.error.statusCode === 404 ? this.pageNotFound : this.otherError
    return {
      title
    }
  }
}
</script>
