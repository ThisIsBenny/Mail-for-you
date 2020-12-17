<i18n>
{
  "en": {
    "title": "Mail for you",
    "messageFieldLabel": "Message",
    "messageFieldErrorRequired": "Message is required.",
    "messageFieldErrorLength": "Message length cannot be longer than 50 characters.",
    "imageUrlFieldLabel": "Image-URL (optional)",
    "imageUrlFieldErrorFormat": "No valid image URL. Only JPEG images are supported.",
    "confettiFieldLabel": "Confetti animation",
    "createButtonLabel": "Create letter",
    "previewButtonLabel": "Preview"
  },
  "de": {
    "title": "Post für dich",
    "messageFieldLabel": "Nachricht",
    "messageFieldErrorRequired": "Nachricht ist erforderlich.",
    "messageFieldErrorLength": "Nachricht darf nicht länger als 50 Zeichen sein.",
    "imageUrlFieldLabel": "Bild-URL (optional)",
    "imageUrlFieldErrorFormat": "Keine gültige Bild-URL. Es werden nur JPEG Bilder unterstützt.",
    "confettiFieldLabel": "Konfetti Animation",
    "createButtonLabel": "Brief erstellen",
    "previewButtonLabel": "Vorschau"
  }
}
</i18n>
<template>
  <v-card class="mt-12">
    <v-row class="mr-2" no-gutters>
      <v-col cols="8">
        <v-card-title class="headline">
          <v-icon left>
            {{ mdiEmail }}
          </v-icon>
          {{ $t('title') }}
        </v-card-title>
      </v-col>
      <v-col cols="4" class="text-right my-auto">
        <v-btn small text depressed :to="switchLocalePath('de')">
          DE
        </v-btn>
        <v-btn small text depressed :to="switchLocalePath('en')">
          EN
        </v-btn>
      </v-col>
    </v-row>
    <v-card-text>
      <v-form v-model="valid">
        <v-text-field
          v-model="message"
          required
          :label="$t('messageFieldLabel')"
          :rules="[v => !!v || $t('messageFieldErrorRequired'), v => v.length <= 50 || $t('messageFieldErrorLength')]"
          :counter="50"
        />
        <v-text-field
          v-model="image"
          :label="$t('imageUrlFieldLabel')"
          :rules="[v => v.length === 0 || /\.(jpg|jpeg)$/.test(v) || $t('imageUrlFieldErrorFormat')]"
        />
        <v-checkbox v-model="confetti" :label="$t('confettiFieldLabel')" />
      </v-form>
    </v-card-text>
    <v-card-actions>
      <v-btn
        class="mr-2"
        small
        color="primary"
        outlined
        :disabled="!valid"
        @click="generateEnvelop()"
      >
        <v-icon left>
          {{ mdiEmail }}
        </v-icon>
        {{ $t('createButtonLabel') }}
      </v-btn>
      <v-btn
        outlined
        small
        :disabled="!valid"
        :href="url"
        target="_blank"
      >
        <v-icon left>
          {{ mdiEye }}
        </v-icon>
        {{ $t('previewButtonLabel') }}
      </v-btn>
    </v-card-actions>
    <v-divider class="mt-4" />
    <v-card-text class="text-center">
      Made with
      <v-icon color="red">
        {{ mdiHeart }}
      </v-icon>
      in Düsseldorf
    </v-card-text>
    <created-dialog :active.sync="dialog" :url="url" />
  </v-card>
</template>

<script>
import { mdiEmail, mdiEye, mdiHeart } from '@mdi/js'
import createdDialog from '../components/createdDialog.vue'
export default {
  cache: true,
  components: { createdDialog },
  data: () => {
    return {
      mdiEmail,
      mdiEye,
      mdiHeart,
      dialog: false,
      valid: false,
      message: '',
      image: '',
      confetti: true
    }
  },
  computed: {
    url: function () {
      if (!process.client) {
        return ''
      }
      const json = JSON.stringify({
        message: this.message,
        image: this.image,
        confetti: this.confetti
      })
      return window.location.toString().replace(/\/$/, '') + '/' + Buffer.from(json).toString('base64')
    }
  },
  methods: {
    generateEnvelop: function () {
      this.dialog = true
    }
  }
}
</script>
