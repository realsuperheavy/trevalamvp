<template>
  <div>
    <h2 class="text-2xl font-bold mb-4">Register</h2>
    <Form @submit="onSubmit">
      <Field name="email" type="email" :rules="emailRule" v-slot="{ field, errors }">
        <input v-bind="field" placeholder="Email" class="input input-bordered w-full max-w-xs mb-2">
        <span class="text-red-500">{{ errors[0] }}</span>
      </Field>
      <Field name="password" type="password" :rules="passwordRule" v-slot="{ field, errors }">
        <input v-bind="field" placeholder="Password" class="input input-bordered w-full max-w-xs mb-2">
        <span class="text-red-500">{{ errors[0] }}</span>
      </Field>
      <button type="submit" class="btn btn-primary">Register</button>
    </Form>
  </div>
</template>

<script setup>
import { Form, Field } from 'vee-validate'
import { getAuth, createUserWithEmailAndPassword } from 'firebase/auth'
import { useRouter } from 'vue-router'

const router = useRouter()

const emailRule = (value) => {
  if (!value) return 'Email is required'
  if (!/^[A-Z0-9._%+-]+@[A-Z0-9.-]+\.[A-Z]{2,4}$/i.test(value)) {
    return 'Invalid email address'
  }
  return true
}

const passwordRule = (value) => {
  if (!value) return 'Password is required'
  if (value.length < 6) return 'Password must be at least 6 characters'
  return true
}

const onSubmit = async (values) => {
  try {
    const auth = getAuth()
    await createUserWithEmailAndPassword(auth, values.email, values.password)
    router.push('/dashboard')
  } catch (error) {
    console.error('Registration failed', error)
    // Handle registration error (show message to user)
  }
}
</script>