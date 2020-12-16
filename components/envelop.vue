<template>
  <div class="envelop" :class="{active: isActive}" @click="toggle()">
    <div class="back">
      <div class="paper">
        <h1>{{ message }}</h1>
        <img v-if="image" class="mt-4" :src="'https://ik.imagekit.io/gvx4saqst2x/tr:w-400/' + image">
      </div>
      <div class="cover" />
      <div class="bottom" />
    </div>
  </div>
</template>

<script>
export default {
  props: {
    message: {
      type: String,
      default: ''
    },
    image: {
      type: String,
      default: null
    }
  },
  data: () => {
    return {
      isActive: false
    }
  },
  methods: {
    toggle: function () {
      this.isActive = !this.isActive
      if (this.isActive) {
        const _this = this
        setTimeout(() => {
          _this.$confetti.start()
        }, 2500)
        setTimeout(() => {
          _this.$confetti.stop()
        }, 30000)
      }
    }
  }
}
</script>
