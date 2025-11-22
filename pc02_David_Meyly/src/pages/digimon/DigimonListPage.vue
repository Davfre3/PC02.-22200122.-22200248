<template>
  <q-page class="digimon-list-page q-pa-md">
    <!-- Banner superior con logo -->
    <div class="banner-wrapper q-mb-lg flex flex-center">
      <img
        class="digimon-logo"
        :src="digimonLogo"
        alt="Digimon Logo"
      />
    </div>

    <!-- Filtros arriba -->
    <div class="filters-wrapper q-mb-md">
      <digimon-filter @update:filters="handleFilterChange" />
    </div>

    <!-- Contenedor de lista -->
    <q-card>
      <q-card-section>
        <div class="row items-center justify-between q-mb-md">
          <div class="text-h5 title-gradient">
            <q-icon name="pets" class="q-mr-sm" />
            Lista de Digimons
          </div>
          <div class="text-subtitle2 text-grey-7">
            {{ filteredDigimons.length }} resultados
          </div>
        </div>

        <!-- Loading -->
        <div v-if="loading" class="row justify-center q-py-xl">
          <q-spinner-dots color="primary" size="50px" />
        </div>

        <!-- Error -->
        <div v-else-if="error" class="text-center q-py-xl">
          <q-icon name="error_outline" size="64px" color="negative" />
          <div class="text-h6 q-mt-md">Error al cargar los Digimons</div>
          <div class="text-body2 text-grey-7 q-mt-sm">{{ error }}</div>
          <q-btn
            flat
            color="primary"
            label="Reintentar"
            icon="refresh"
            class="q-mt-md"
            @click="fetchDigimons"
          />
        </div>

        <!-- Lista de Digimons -->
        <div v-else-if="filteredDigimons.length > 0" class="row q-col-gutter-md">
          <div
            v-for="digimon in filteredDigimons"
            :key="digimon.name"
            class="col-12 col-sm-6 col-md-4 col-lg-3"
          >
            <digimon-card :digimon="digimon" />
          </div>
        </div>

        <!-- Sin resultados -->
        <div v-else class="text-center q-py-xl">
          <q-icon name="search_off" size="64px" color="grey-5" />
          <div class="text-h6 q-mt-md text-grey-7">No se encontraron Digimons</div>
          <div class="text-body2 text-grey-6 q-mt-sm">
            Intenta ajustar los filtros de búsqueda
          </div>
        </div>
      </q-card-section>
    </q-card>
  </q-page>
</template>

<script setup>
import { ref, computed, onMounted } from 'vue'
import digimonLogo from '../../imagen/digimon-logo.png'
import { api } from 'boot/axios'
import DigimonCard from 'components/digimon/DigimonCard.vue'
import DigimonFilter from 'components/digimon/DigimonFilter.vue'

const digimons = ref([])
const loading = ref(false)
const error = ref(null)
const filters = ref({
  name: '',
  level: null
})

const filteredDigimons = computed(() => {
  let result = digimons.value

  // Filtrar por nombre
  if (filters.value.name) {
    result = result.filter(digimon =>
      digimon.name.toLowerCase().includes(filters.value.name.toLowerCase())
    )
  }

  // Filtrar por nivel
  if (filters.value.level) {
    result = result.filter(digimon =>
      digimon.level === filters.value.level
    )
  }

  return result
})

const fetchDigimons = async () => {
  loading.value = true
  error.value = null

  try {
    const response = await api.get('https://digimon-api.vercel.app/api/digimon')
    digimons.value = response.data
  } catch (err) {
    error.value = err.message || 'Error desconocido al cargar los datos'
    console.error('Error fetching digimons:', err)
  } finally {
    loading.value = false
  }
}

const handleFilterChange = (newFilters) => {
  filters.value = { ...newFilters }
}

onMounted(() => {
  fetchDigimons()
})
</script>

<style scoped lang="scss">
.digimon-list-page {
  max-width: 1400px;
  margin: 0 auto;
}

.banner-wrapper {
  background: radial-gradient(circle at center, #1e3a8a 0%, #0d1117 70%);
  border-radius: 12px;
  padding: 24px 16px;
  position: relative;
  overflow: hidden;
  box-shadow: 0 8px 24px -8px rgba(0,0,0,0.5);
}

.banner-wrapper::before {
  content: '';
  position: absolute;
  inset: 0;
  background: linear-gradient(120deg, rgba(255,255,255,0.08), rgba(255,255,255,0));
  pointer-events: none;
}

.digimon-logo {
  max-width: 480px;
  width: 100%;
  filter: drop-shadow(0 4px 8px rgba(0,0,0,0.4));
}

.filters-wrapper {
  position: relative;
}

.title-gradient {
  background: linear-gradient(90deg,#ff9800,#ffeb3b);
  -webkit-background-clip: text;
  background-clip: text;
  color: transparent;
  font-weight: 700;
}
</style>
