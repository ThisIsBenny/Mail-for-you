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
        <v-switch
          v-model="confetti"
          inset
          label="Konfetti-Animation beim öffnen"
        />
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
    <v-divider class="mt-4" />
    <v-card-text class="text-center">
      Made with
      <v-icon color="red">
        {{ mdiHeart }}
      </v-icon>
      in Düsseldorf
    </v-card-text>
    <v-dialog
      v-model="dialog"
      max-width="500"
    >
      <v-card>
        <v-card-title class="title">
          Brief erstellt
        </v-card-title>

        <v-card-text>
          <p>Dein Brief ist erstellt und bereit zum versenden:</p>
          <v-btn text color="primary" @click="addToClipboard(url)">
            <v-icon left>
              {{ mdiClipboardOutline }}
            </v-icon>
            Link kopieren
          </v-btn>
          <br>
          <v-btn text color="primary" :href="'sms:&body=Ich habe einen Brief für dich: ' + url">
            <v-icon left>
              {{ mdiMessageText }}
            </v-icon>
            Als SMS versenden
          </v-btn>
          <br>
          <v-btn text color="primary" :href="'whatsapp://send?text=Ich habe einen Brief für dich: ' + url">
            <v-icon left>
              {{ mdiWhatsapp }}
            </v-icon>
            Als WhatsApp versenden
          </v-btn>
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
import { mdiEmail, mdiEye, mdiCheck, mdiHeart, mdiClipboardOutline, mdiMessageText, mdiWhatsapp } from '@mdi/js'
export default {
  data: () => {
    return {
      mdiEmail,
      mdiEye,
      mdiCheck,
      mdiHeart,
      mdiClipboardOutline,
      mdiMessageText,
      mdiWhatsapp,
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
      image: '',
      confetti: true
    }
  },
  computed: {
    url: function () {
      const json = JSON.stringify({
        message: this.message,
        image: this.image,
        confetti: this.confetti
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
