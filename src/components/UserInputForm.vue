<template>
  <div class="grid grid-cols-1 md:grid-cols-3 gap-4 mb-6 p-4 border border-gray-200 rounded-lg shadow">
    <div>
      <label for="solarDate" class="block mb-2 font-medium text-gray-700">陽曆生日:</label>
      <input type="date" id="solarDate" v-model="solarDate" class="input-field">
    </div>
    <div>
      <label for="timeIndex" class="block mb-2 font-medium text-gray-700">出生時辰:</label>
      <select id="timeIndex" v-model.number="timeIndex" class="input-field">
        <option value="0">早子時 (00:00 - 00:59)</option>
        <option value="1">丑時 (01:00 - 02:59)</option>
        <option value="2">寅時 (03:00 - 04:59)</option>
        <option value="3">卯時 (05:00 - 06:59)</option>
        <option value="4">辰時 (07:00 - 08:59)</option>
        <option value="5">巳時 (09:00 - 10:59)</option>
        <option value="6">午時 (11:00 - 12:59)</option>
        <option value="7">未時 (13:00 - 14:59)</option>
        <option value="8">申時 (15:00 - 16:59)</option>
        <option value="9">酉時 (17:00 - 18:59)</option>
        <option value="10">戌時 (19:00 - 20:59)</option>
        <option value="11">亥時 (21:00 - 22:59)</option>
        <option value="12">晚子時 (23:00 - 23:59)</option>
      </select>
    </div>
    <div>
      <label for="gender" class="block mb-2 font-medium text-gray-700">性別:</label>
      <select id="gender" v-model="gender" class="input-field">
        <option value="男">男</option>
        <option value="女">女</option>
      </select>
    </div>
    <div class="md:col-span-3">
      <button @click="handleSubmit" class="w-full bg-blue-500 hover:bg-blue-600 text-white font-semibold py-2 px-4 rounded-md transition duration-300">
        計算星盤
      </button>
    </div>
  </div>
</template>

<script setup>
import { ref } from 'vue';

const solarDate = ref('1990-01-01');
const timeIndex = ref(6); // 午時 as default
const gender = ref('男');

const emit = defineEmits(['calculate']);

function handleSubmit() {
  if (!solarDate.value) {
    alert('請選擇陽曆生日。'); // 简单提示，后续可以改进为更友好的UI
    return;
  }
  emit('calculate', {
    solarDate: solarDate.value,
    timeIndex: timeIndex.value,
    gender: gender.value,
  });
}
</script>

<style scoped>
.input-field {
  width: 100%;
  padding: 0.75rem;
  margin-bottom: 1rem;
  border: 1px solid #d1d5db; /* Tailwind gray-300 */
  border-radius: 0.375rem; /* Tailwind rounded-md */
  box-sizing: border-box;
  transition: border-color 0.3s, box-shadow 0.3s;
}
.input-field:focus {
  outline: none;
  border-color: #3b82f6; /* Tailwind blue-500 */
  box-shadow: 0 0 0 3px rgba(59, 130, 246, 0.25); /* Tailwind ring-blue-200 */
}
</style> 