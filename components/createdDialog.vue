<i18n>
{
  "en": {
    "title": "Mail created",
    "text": "Your mail is created and ready to send:",
    "copyLink": "Copy link",
    "copiedMessage": "URL was added to the clipboard.",
    "shareLink": "Share link",
    "shareTitle": "Mail for you",
    "sms": "Send as SMS",
    "whatsapp": "Send as WhatsApp",
    "messageText": "I have a mail for you:",
    "closeButton": "Close"
  },
  "de": {
    "title": "Brief erstellt",
    "text": "Dein Brief ist erstellt und bereit zum versenden:",
    "copyLink": "Link kopieren",
    "copiedMessage": "URL wurde zur Zwischenablage hinzugefügt.",
    "shareLink": "Link teilen",
    "shareTitle": "Post für dich",
    "sms": "Als SMS versenden",
    "whatsapp": "Als WhatsApp versenden",
    "messageText": "Ich habe einen Brief für dich:",
    "closeButton": "Schließen"
  }
}
</i18n>
<template>
  <v-dialog
    v-model="active"
    max-width="500"
  >
    <v-card>
      <v-card-title class="title">
        {{ $t('title') }}
      </v-card-title>

      <v-card-text>
        <p>{{ $t('text') }}</p>
        <v-row>
          <v-btn text color="primary" @click="clipboard(url)">
            <v-icon left>
              {{ mdiClipboardOutline }}
            </v-icon>
            {{ $t('copyLink') }}
          </v-btn>
        </v-row>
        <v-row>
          <v-btn v-if="shareSupport" text color="primary" @click="share(url)">
            <v-icon left>
              {{ mdiExportVariant }}
            </v-icon>
            {{ $t('shareLink') }}
          </v-btn>
        </v-row>
        <v-row>
          <v-btn text color="primary" :href="'sms:&body=' + $t('messageText') + ' ' + url">
            <v-icon left>
              {{ mdiMessageText }}
            </v-icon>
            {{ $t('sms') }}
          </v-btn>
        </v-row>
        <v-row>
          <v-btn text color="primary" :href="'whatsapp://send?text=' + $t('messageText') + ' ' + url">
            <v-icon left>
              {{ mdiWhatsapp }}
            </v-icon>
            {{ $t('whatsapp') }}
          </v-btn>
        </v-row>
      </v-card-text>

      <v-divider />

      <v-card-actions>
        <v-spacer />
        <v-btn
          color="primary"
          text
          @click="$emit('update:active', false)"
        >
          {{ $t('closeButton') }}
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
      {{ $t('copiedMessage') }}
    </v-snackbar>
  </v-dialog>
</template>

<script>
import { mdiCheck, mdiClipboardOutline, mdiMessageText, mdiWhatsapp, mdiExportVariant } from '@mdi/js'

export default {
  props: {
    active: {
      type: Boolean,
      default: false
    },
    url: {
      type: String,
      default: ''
    }
  },
  data: () => {
    return {
      mdiCheck,
      mdiClipboardOutline,
      mdiExportVariant,
      mdiMessageText,
      mdiWhatsapp,
      addedToClipboard: false
    }
  },
  computed: {
    shareSupport: function () {
      return navigator !== undefined && navigator.share !== undefined
    }
  },
  methods: {
    clipboard: function (text) {
      navigator.clipboard.writeText(text)
      this.addedToClipboard = true
    },
    share: async function (url) {
      try {
        await navigator.share({
          title: this.$t('shareTitle'),
          url
        })
      } catch (e) {
      }
    }
  }
}
</script>
