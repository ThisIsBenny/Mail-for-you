<template>
  <v-card class="mt-12">
    <v-card-title class="headline">
      <v-icon left>
        {{ mdiEmail }}
      </v-icon>
      Post für dich
    </v-card-title>
    <v-card-text>
      <v-form v-model="valid">
        <v-text-field v-model="message" required label="Nachricht" :rules="messageRules" :counter="50" />
        <v-text-field v-model="image" label="Bild-URL (optional)" :rules="imageRules" />
      </v-form>
    </v-card-text>
    <v-divider />
    <v-card-actions>
      <v-btn
        class="mr-2"
        small
        color="success"
        outlined
        :disabled="!valid"
        @click="generateEnvelop()"
      >
        <v-icon left>
          {{ mdiEmail }}
        </v-icon>
        Brief erstellen
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
        Vorschau
      </v-btn>
    </v-card-actions>
    <v-dialog
      v-model="dialog"
      max-width="500"
    >
      <v-card>
        <v-card-title class="title">
          Brief erstellt
        </v-card-title>

        <v-card-text>
          Dein Brief ist erstellt und bereit zum versenden:
          <v-row>
            <v-col cols="3">
              URL:
            </v-col>
            <v-col cols="9" @click="addToClipboard(url)">
              {{ url }}
            </v-col>
          </v-row>
        </v-card-text>

        <v-divider />

        <v-card-actions>
          <v-spacer />
          <v-btn
            color="primary"
            text
            @click="dialog = false"
          >
            Schließen
          </v-btn>
        </v-card-actions>
      </v-card>
      <v-snackbar
        v-model="addedToClipboard"
        timeout="5000"
        top
      >
        <v-icon left color="success">
          {{ mdiCheck }}
        </v-icon>
        URL wurde zur Zwischenablage hinzugefügt.
      </v-snackbar>
    </v-dialog>
  </v-card>
</template>

<script>
import { mdiEmail, mdiEye, mdiCheck } from '@mdi/js'
export default {
  data: () => {
    return {
      mdiEmail,
      mdiEye,
      mdiCheck,
      dialog: false,
      addedToClipboard: false,
      valid: false,
      messageRules: [
        v => !!v || 'Nachricht ist erforderlich',
        v => v.length <= 50 || 'Nachricht darf nicht länger als 50 Zeichen sein'
      ],
      imageRules: [
        v => v.length === 0 || /\.(jpg|jpeg)$/.test(v) || 'Keine gültige Bild-URL. Es werden nur JPEG Bilder unterstützt.'
      ],
      message: '',
      image: ''
    }
  },
  computed: {
    url: function () {
      const json = JSON.stringify({
        message: this.message,
        image: this.image
      })
      return window.location.protocol + '//' + window.location.host + '/' + Buffer.from(json).toString('base64')
    }
  },
  methods: {
    generateEnvelop: function () {
      this.dialog = true
    },
    addToClipboard: function (text) {
      navigator.clipboard.writeText(text)
      this.addedToClipboard = true
    }
  }
}
</script>
