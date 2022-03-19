<template>
  <div id="employee-form">
    <v-form>
      <v-text-field
          ref="first"
          v-model="employee.name"
          label="氏名"
          required
          :rules="nameRules"
          @focus="clearStatus"
      ></v-text-field>
      <v-text-field
          v-model="employee.email"
          label="メールアドレス"
          required
          :rules="emailRules"
      ></v-text-field>
      <v-btn @click="submit"> 追加</v-btn>

    </v-form>
  </div>
</template>

<script>
export default {
  name: 'employee-form',
  data() {
    return {
      mes: "",
      submitting: false,
      error: false,
      success: false,
      employee: {
        name: '',
        email: '',
      },
      nameRules: [
        v => !!v || 'Name is required',
        v => v.length <= 10 || 'Name must be less than 10 characters',
      ],
      emailRules: [
        v => !!v || 'E-mail is required',
        v => /.+@.+/.test(v) || 'E-mail must be valid',
      ],

    }
  },
  methods: {
    submit() {
      this.submitting = true
      this.clearStatus()

      if (this.invalidName || this.invalidEmail) {
        this.error = true
        return
      }
      this.$emit('add:employee', this.employee)
      this.$refs.first.focus()

      this.employee = {
        name: '',
        email: '',
      }
      this.error = false
      this.success = true
      this.submitting = false
    },
    clearStatus() {
      this.success = false
      this.error = false
    }
  },
  computed: {
    invalidName() {
      return this.employee.name === ''
    },
    invalidEmail() {
      return this.employee.email === ''
    }
  }

}
</script>

<style scoped>
</style>