<template>
  <v-flex>
    <v-row>
      <v-col cols="6">半分</v-col>
            <v-col cols="6">半分</v-col>
    </v-row>
    
  </v-flex>
</template>
<script>
import { validationMixin } from 'vuelidate'
import { required, maxLength, email, minLength } from 'vuelidate/lib/validators'
export default {
  mixins: [validationMixin],
  layout: 'auth',
  validations: {
    email: { required, email, maxLength: maxLength(23), minLength: minLength(23) },
    password: { required, minLength: minLength(8) },
  },

  data: () => ({
    show1: false,
    email: '',
    password: '',
    user: {},
  }),
  // email: '1001999@s..ac.jp',
  // password: 'test-2022Aso',
  computed: {
    emailErrors() {
      const errors = []
      if (!this.$v.email.$dirty) return errors
      !this.$v.email.email && errors.push('Must be valid e-mail')
      !this.$v.email.required && errors.push('E-mail is required')
      return errors
    },
    passwordErrors() {
      const errors = []
      if (!this.$v.password.$dirty) return errors
      !this.$v.password.minLength && errors.push('Password must be at least 8 characters')
      !this.$v.password.required && errors.push('Password is required')
      return errors
    },
  },

  methods: {
    async submit() {
      this.$v.$touch()
      const sendSearch = []
      sessionStorage.setItem('sendSearch', JSON.stringify(sendSearch))
      sessionStorage.setItem('searchFlg', 0)

      this.user = {
        email: this.email,
        password: this.password,
      }

      await this.$axios.post(`${this.$urls.API}/auth`, this.user).then(async response => {
        await this.$store.commit('restoreLogin', response.data)
      })
    },

    clear() {
      this.$v.$reset()
      this.name = ''
      this.email = ''
      this.select = null
      this.checkbox = false
    },
  },
}
</script>
<style lang="scss"></style>
