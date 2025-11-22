<template>
  <div class="filter-bar q-pa-sm q-mb-sm">
    <div class="row items-center q-col-gutter-sm">
      <div class="col-12 col-md-4">
        <q-input
          v-model="localFilters.name"
          dense
          outlined
          label="Nombre"
          placeholder="Ej: Agumon"
          clearable
          @update:model-value="emitFilters"
        >
          <template #prepend>
            <q-icon name="search" />
          </template>
        </q-input>
      </div>
      <div class="col-12 col-md-4">
        <q-select
          v-model="localFilters.level"
          dense
          outlined
          label="Nivel"
          :options="levelOptions"
          clearable
          @update:model-value="emitFilters"
        >
          <template #prepend>
            <q-icon name="stars" />
          </template>
        </q-select>
      </div>
      <div class="col-12 col-md-4 flex items-center">
        <q-btn
          class="q-mr-sm"
          color="primary"
          unelevated
          icon="filter_list"
          label="Filtrar"
          @click="emitFilters"
        />
        <q-btn
          v-if="hasActiveFilters"
          flat
          color="negative"
          icon="clear"
          label="Limpiar"
          @click="clearFilters"
        />
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref, computed, defineEmits } from 'vue'

const emit = defineEmits(['update:filters'])

const localFilters = ref({
  name: '',
  level: null
})

const levelOptions = [
  'Fresh',
  'In Training',
  'Rookie',
  'Champion',
  'Ultimate',
  'Mega',
  'Armor'
]

const hasActiveFilters = computed(() => {
  return localFilters.value.name !== '' || localFilters.value.level !== null
})

const emitFilters = () => {
  emit('update:filters', {
    name: localFilters.value.name,
    level: localFilters.value.level
  })
}

const clearFilters = () => {
  localFilters.value.name = ''
  localFilters.value.level = null
  emitFilters()
}
</script>

<style scoped lang="scss">
.filter-bar {
  background: linear-gradient(90deg, rgba(0,0,0,0.25), rgba(255,255,255,0.05));
  backdrop-filter: blur(4px);
  border: 1px solid rgba(255,255,255,0.1);
  border-radius: 12px;
  box-shadow: 0 4px 12px -4px rgba(0,0,0,0.4);
}
</style>
