<script setup>
import FilePicker from './FilePicker.vue'
defineProps({
  fileTypes: String
})
</script>

<template>
  <form ref="form">
    <fieldset>
      <legend>Personal information</legend>
      <label for="email">Email</label>
      <input v-model="email" id="email" name="email" type="email" required placeholder="john.doe@yahoo.com"/>
      <label for="confirm-email">Confirm email</label>
      <input v-model="confirmEmail" id="confirm-email" name="confirm-email" type="email" required placeholder="john.doe@yahoo.com"/>
    </fieldset>
    <fieldset>
      <legend>Pick file</legend>
      <FilePicker :file-types="fileTypes" @raise-file-picked="handleFilePicked"/>
    </fieldset>
    <fieldset>
      <legend>Upload</legend>
      <button type="button" :disabled="!isValid()" @click="submit">Upload</button>
    </fieldset>
    <progress v-if="loading" id="progress-bar" aria-label="Content loading…"></progress>
    <p v-if="error">An error has occured! Please try again later.</p>
    <p v-if="text">{{ text }}</p>
  </form>
</template>

<script>
export default {
  emits: ['raise-file-uploaded'],
  data() {
    return {
      email: localStorage.getItem("email"),
      confirmEmail: localStorage.getItem("email"),
      fileData: "",
      text: "",
      loading: false,
      error: false,
    }
  },
  methods: {
    handleFilePicked(fileData) {
      this.fileData = fileData
    },
    isValid() {
      if (!this.fileData) {
        return false
      }
      if (!this.confirmEmail || !this.email) {
        return false
      }
      if (!this.confirmEmail === this.email) {
        return false
      }

      return true
    },
    async submit() {
      try {
        this.loading = true
        await axios.post(`https://ry8kes9z1f.execute-api.us-east-1.amazonaws.com/kjm-reporting-portal-ocr`, {
          data
        })

        this.text = response.data.responses[0].fullTextAnnotation.text

        localStorage.setItem("email", email)
      } catch (err) {
        this.error = err
      } finally {
        this.loading = false
      }
    }
  }
}
</script>
<style scoped>
progress {
  width: 100%;
  accent-color: var(--color-primary);
}
</style>