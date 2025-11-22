<template>
  <div class="trading-card">
    <div class="tc-inner">
      <div class="tc-title">{{ digimon.name }}</div>
      <div class="tc-image-wrapper">
        <q-img
          :src="digimon.img"
          :alt="digimon.name"
          class="tc-image"
          ratio="1"
          spinner-color="primary"
        >
          <template #error>
            <div class="absolute-full flex flex-center bg-grey-3 text-grey-7">
              <q-icon name="broken_image" size="64px" />
            </div>
          </template>
        </q-img>
      </div>
      <div class="tc-footer">
        <q-chip
          :color="getLevelColor(digimon.level)"
          text-color="white"
          icon="stars"
          dense
        >
          {{ digimon.level }}
        </q-chip>
      </div>
    </div>
  </div>
</template>

<script setup>
// La imagen se referencia directamente en CSS, no se requiere import JS
defineProps({
  digimon: {
    type: Object,
    required: true,
    validator: (value) => {
      return value.name && value.img && value.level
    }
  }
})

const getLevelColor = (level) => {
  const colors = {
    'Fresh': 'grey-5',
    'In Training': 'blue-grey-5',
    'Rookie': 'green',
    'Champion': 'blue',
    'Ultimate': 'deep-purple',
    'Mega': 'red',
    'Armor': 'amber'
  }
  return colors[level] || 'primary'
}
</script>

<style scoped lang="scss">
.trading-card {
  position: relative;
  width: 100%;
  aspect-ratio: 3/4.5;
  background: url("../../imagen/digimon-card-frame.jpg") center/cover no-repeat,
              linear-gradient(135deg,#c084fc,#60a5fa,#34d399,#fbbf24,#f472b6);
  background-blend-mode: overlay;
  background-size: cover;
  padding: 6px;
  border-radius: 16px;
  box-shadow: 0 8px 24px -6px rgba(0,0,0,0.5);
  display: flex;
}

.tc-inner {
  background: #fafafa;
  border-radius: 12px;
  flex: 1;
  display: flex;
  flex-direction: column;
  overflow: hidden;
  position: relative;
}

.tc-title {
  text-align: center;
  font-weight: 700;
  font-size: 0.95rem;
  padding: 6px 4px;
  letter-spacing: 0.5px;
  background: linear-gradient(90deg,#1e3a8a,#0d47a1,#1e3a8a);
  color: #fff;
  text-shadow: 0 1px 2px rgba(0,0,0,0.6);
}

.tc-image-wrapper {
  flex: 1;
  padding: 8px 8px 4px;
  display: flex;
}

.tc-image {
  border: 2px solid #222;
  border-radius: 8px;
  background: radial-gradient(circle at 30% 30%, #ffffff, #e0e7ff);
}

.tc-footer {
  padding: 6px 4px 10px;
  text-align: center;
  background: linear-gradient(180deg,#e2e8f0,#f8fafc);
}

.trading-card:hover {
  transform: translateY(-4px);
  transition: transform 0.25s ease;
}
</style>
