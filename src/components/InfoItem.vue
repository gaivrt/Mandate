<template>
  <div class="info-item-vue">
    <p class="info-item-title-vue">{{ title }}:</p>
    <p class="info-item-value-vue" v-if="!isObject">{{ value || '-' }}</p>
    <pre v-else class="info-item-value-vue bg-white p-2 rounded text-xs">{{ JSON.stringify(value, null, 2) }}</pre>
  </div>
</template>

<script setup>
const props = defineProps({
  title: String,
  value: [String, Number, Object, Array],
  isObject: {
    type: Boolean,
    default: false
  }
});
</script>

<style scoped>
.info-item-vue {
  background-color: #f9fafb; /* Tailwind gray-50 */
  padding: 1rem;
  border-radius: 0.5rem; /* Tailwind rounded-lg */
  border: 1px solid #e5e7eb; /* Tailwind gray-200 */
}

.info-item-title-vue {
  font-weight: 600;
  color: #111827; /* Tailwind gray-900 */
  margin-bottom: 0.25rem;
}

.info-item-value-vue {
  color: #4b5563; /* Tailwind gray-600 */
  font-size: 0.875rem; /* Tailwind text-sm */
  word-break: break-all;
}
</style> 