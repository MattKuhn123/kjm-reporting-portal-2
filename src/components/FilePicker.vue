<script setup>
defineProps({
  fileTypes: String
})
</script>

<template>
  <button type="button" @click="simulateClick" :class="{ error: hasError }">
    Click here to upload you attendance sheet!
    <span v-if="hasError">
      Could not read file!
    </span>
  </button>
  <input ref="fileInput" :accept="fileTypes" name="file" type="file" @change="handleInputChange" hidden/>
</template>

<script>
export default {
  emits: ['raise-file-picked'],
  data() {
    return {
      hasError: false
    }
  },
  methods: {
    simulateClick() {
      this.$refs.fileInput.click()
    },
    handleInputChange(event) {
      this.hasError = false;
      if (!event.target.files) {
        return;
      }

      const reader = new FileReader()
      reader.onload = async (e) => {
        const fileData = reader.result.replace("data:image/jpeg;base64,", "")
        this.$emit('raise-file-picked', fileData)
      }

      reader.onerror = () => {
        this.hasError = true;
      }

      reader.readAsDataURL(event.target.files[0])
    }
  }
}
</script>
