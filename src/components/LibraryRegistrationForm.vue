<template>
  <div class="container mt-5">
    <div class="row">
      <div class="col-12 col-md-8 offset-md-2">
        <h1 class="text-center">User Information Form</h1>

        <form @submit.prevent="submitForm">
          <!-- Username and Password -->
          <div class="row mb-3">
            <div class="col-12 col-md-6">
              <label for="username" class="form-label">Username</label>

              <input
                type="text"
                class="form-control"
                id="username"
                required
                v-model="formData.username"
                @blur="validateUsername"
              />

              <div v-if="usernameError" class="text-danger">
                {{ usernameError }}
              </div>
            </div>

            <div class="col-12 col-md-6">
              <label for="password" class="form-label">Password</label>

              <input
                type="password"
                class="form-control"
                id="password"
                minlength="4"
                maxlength="10"
                required
                v-model="formData.password"
                @blur="validatePassword"
              />

              <div v-if="passwordError" class="text-danger">
                {{ passwordError }}
              </div>
            </div>
          </div>

          <!-- Australian Resident and Gender -->
          <div class="row mb-3">
            <div class="col-12 col-md-6">
              <div class="form-check">
                <input
                  type="checkbox"
                  class="form-check-input"
                  id="isAustralian"
                  required
                  v-model="formData.isAustralian"
                />

                <label class="form-check-label" for="isAustralian">
                  Australian Resident?
                </label>
              </div>
            </div>

            <div class="col-12 col-md-6">
              <label for="gender" class="form-label">Gender</label>

              <select
                class="form-select"
                id="gender"
                required
                v-model="formData.gender"
              >
                <option value="" disabled>Select gender</option>
                <option value="male">Male</option>
                <option value="female">Female</option>
                <option value="other">Other</option>
              </select>
            </div>
          </div>

          <!-- Reason -->
          <div class="mb-3">
            <label for="reason" class="form-label">
              Reason for joining
            </label>

            <textarea
              class="form-control"
              id="reason"
              rows="3"
              required
              v-model="formData.reason"
              @blur="validateReason"
            ></textarea>

            <div v-if="reasonError" class="text-danger">
              {{ reasonError }}
            </div>
          </div>

          <!-- Buttons -->
          <div class="text-center">
            <button
              type="submit"
              class="btn btn-primary me-2"
            >
              Submit
            </button>

            <button
              type="button"
              class="btn btn-secondary"
              @click="clearForm"
            >
              Clear
            </button>
          </div>
        </form>

        <!-- PrimeVue DataTable -->
        <div class="mt-5" v-if="submittedUsers.length">
          <h2 class="text-center mb-3">Submitted Users</h2>

          <DataTable
            :value="submittedUsers"
            stripedRows
            showGridlines
            tableStyle="min-width: 50rem"
          >
            <Column field="username" header="Username"></Column>

            <Column field="password" header="Password"></Column>

            <Column header="Australian Resident">
              <template #body="slotProps">
                {{ slotProps.data.isAustralian ? 'Yes' : 'No' }}
              </template>
            </Column>

            <Column field="gender" header="Gender"></Column>

            <Column field="reason" header="Reason"></Column>
          </DataTable>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref } from 'vue'

import DataTable from 'primevue/datatable'
import Column from 'primevue/column'

const formData = ref({
  username: '',
  password: '',
  isAustralian: false,
  reason: '',
  gender: ''
})

const submittedUsers = ref([])

const usernameError = ref('')
const passwordError = ref('')
const reasonError = ref('')

// Username validation
const validateUsername = () => {
  if (formData.value.username.length < 3) {
    usernameError.value =
      'Username must be at least 3 characters.'
    return false
  }

  usernameError.value = ''
  return true
}

// Password validation
const validatePassword = () => {
  if (formData.value.password.length < 8) {
    passwordError.value =
      'Password must be at least 8 characters.'
    return false
  }

  passwordError.value = ''
  return true
}

// Reason validation
const validateReason = () => {
  if (formData.value.reason.length < 10) {
    reasonError.value =
      'Reason must be at least 10 characters.'
    return false
  }

  reasonError.value = ''
  return true
}

// Submit form
const submitForm = () => {
  const usernameValid = validateUsername()
  const passwordValid = validatePassword()
  const reasonValid = validateReason()

  if (
    !usernameValid ||
    !passwordValid ||
    !reasonValid
  ) {
    return
  }

  submittedUsers.value.push({
    ...formData.value
  })

  clearForm()
}

// Clear form
const clearForm = () => {
  formData.value = {
    username: '',
    password: '',
    isAustralian: false,
    reason: '',
    gender: ''
  }

  usernameError.value = ''
  passwordError.value = ''
  reasonError.value = ''
}
</script>