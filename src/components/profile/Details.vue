<template>
  <form novalidate @submit.prevent="$emit('submit')">
    <v-text-field
      v-model="form.name"
      :error-messages="nameErrors"
      name="name"
      label="Workspace name"
      :disabled="loading"
      @input="$v.form.name.$touch()"
      @blur="$v.form.name.$touch()"
    />
    <v-text-field
      v-model="form.url"
      :error-messages="urlErrors"
      name="url"
      label="Tracker URL"
      :disabled="loading"
      @input="$v.form.url.$touch()"
      @blur="$v.form.url.$touch()"
    />
    <v-text-field
      v-model="form.apiKey"
      :error-messages="apiKeyErrors"
      name="apiKey"
      label="API access key"
      :type="showApiKey ? 'text' : 'password'"
      :append-icon="showApiKey ? 'visibility_off' : 'visibility'"
      counter="40"
      :disabled="loading"
      @input="$v.form.apiKey.$touch()"
      @blur="$v.form.apiKey.$touch()"
      @click:append="showApiKey = !showApiKey"
    />
  </form>
</template>

<script>
import profileValidation from '@/mixins/validations/profile'

export default {
  mixins: [profileValidation],
  props: {
    value: {
      type: Object,
      default: () => {}
    },
    loading: {
      type: Boolean,
      default: () => false
    }
  },
  data: () => ({
    showApiKey: false
  }),
  computed: {
    isValid() {
      return (
        this.nameErrors.length === 0 ||
        this.urlErrors.length === 0 ||
        this.apiKeyErrors.length === 0
      )
    }
  },
  watch: {
    form: {
      handler(newValue) {
        this.value = Object.assign(this.value, newValue)
      },
      deep: true
    },
    value: {
      handler(newValue) {
        this.form = Object.assign(this.form, newValue)
      },
      deep: true
    }
  },
  methods: {
    validate() {
      this.$v.$touch()
      return this.isValid
    }
  }
}
</script>
