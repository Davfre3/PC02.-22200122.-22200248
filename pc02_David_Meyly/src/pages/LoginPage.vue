<template>
  <q-page class="flex flex-center">
    <q-card class="q-pa-md" style="width: 400px; max-width: 90vw;">
      <q-card-section>
        <div class="text-h5 text-center q-mb-md">Inicio de sesión</div>
        
        <!-- Logo de Digimon -->
        <div class="flex flex-center q-mb-md">
          <img 
            src="../imagen/Digimon-Logo-PNG-Clipart-Background.png" 
            alt="Digimon Logo" 
            style="max-width: 200px; height: auto;"
          />
        </div>
        
        <q-input
          v-model="email"
          label="Email"
          type="email"
          outlined
          class="q-mb-md"
        />
        
        <q-input
          v-model="password"
          label="Password"
          :type="showPassword ? 'text' : 'password'"
          outlined
          class="q-mb-md"
        >
          <template v-slot:append>
            <q-icon
              :name="showPassword ? 'visibility_off' : 'visibility'"
              class="cursor-pointer"
              @click="showPassword = !showPassword"
            />
          </template>
        </q-input>
        
        <q-btn
          label="Iniciar Sesión"
          color="primary"
          class="full-width"
          @click="doLogin"
          :loading="loading"
        />
        
        <!-- Mensaje de éxito -->
        <div v-if="successMessage" class="text-positive q-mt-md text-center text-weight-bold">
          {{ successMessage }}
        </div>
        
        <!-- Mensaje de error -->
        <div v-if="errorMessage" class="text-negative q-mt-md text-center text-weight-bold">
          {{ errorMessage }}
        </div>
      </q-card-section>
    </q-card>
  </q-page>
</template>

<script setup>
import { ref } from 'vue'
import { useRouter } from 'vue-router'

const router = useRouter()

const email = ref('')
const password = ref('')
const showPassword = ref(false)
const errorMessage = ref('')
const successMessage = ref('')
const loading = ref(false)

const doLogin = async () => {
  errorMessage.value = ''
  successMessage.value = ''
  loading.value = true
  
  try {
    const response = await fetch('https://storedb-api.onrender.com/node-api/users/signin', {
      method: 'POST',
      headers: {
        'accept': '*/*',
        'Content-Type': 'application/json'
      },
      body: JSON.stringify({
        email: email.value,
        password: password.value
      })
    })
    
    const data = await response.json()
    
    if (response.ok) {
      // Guardar la respuesta completa en localStorage
      localStorage.setItem('user', JSON.stringify(data))
      
      // Mostrar la data en consola
      console.log('Login exitoso:', data)
      console.log('Email:', data.email)
      console.log('ID:', data.id)
      console.log('Token:', data.token)
      
      // Mostrar mensaje de éxito
      successMessage.value = 'Inicio de sesión exitoso'
      
      // Redirigir a /digimon después de 1 segundo
      setTimeout(() => {
        router.push('/digimon')
      }, 1000)
    } else {
      errorMessage.value = 'Credenciales incorrectas'
    }
  } catch (error) {
    console.error('Error en el login:', error)
    errorMessage.value = 'Credenciales incorrectas'
  } finally {
    loading.value = false
  }
}
</script>

<style scoped>
</style>
