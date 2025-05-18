<template>
  <div id="app-container" class="container mx-auto p-4 bg-gray-100 min-h-screen">
    <h1 class="text-3xl font-bold text-center mb-8 text-gray-800">紫微斗數星盤 Vue (iztro.js)</h1>
    
    <UserInputForm @calculate="handleAstrolabeCalculation" />

    <div v-if="errorMessage" class="error-message">
      {{ errorMessage }}
    </div>

    <div v-if="isLoading" class="text-center my-4">
      <p>正在計算星盤...</p>
      <!-- 可以添加一个加载动画 -->
    </div>

    <div v-if="currentAstrolabeData && !isLoading" id="result" class="mt-4">
      <!-- <div id="resultTitle" class="section-title">
        星盤資料
      </div> -->
      <AstrolabeDisplay :astrolabe-data="currentAstrolabeData" />
      <!-- <p>星盘数据已加载，等待显示组件。</p>
      <pre class="raw-json">{{ JSON.stringify(currentAstrolabeData, null, 2) }}</pre> -->
    </div>

  </div>
</template>

<script setup>
import { ref } from 'vue';
import UserInputForm from './components/UserInputForm.vue';
import AstrolabeDisplay from './components/AstrolabeDisplay.vue'; // 新增引入
import { astro } from 'iztro'; // Correctly imported as astro

const currentAstrolabeData = ref(null);
const isLoading = ref(false);
const errorMessage = ref('');

// 从原 HTML 复制过来的部分样式类名，确保 Tailwind 能应用
const sectionTitleClass = "text-xl font-semibold mb-3 text-gray-700 section-title"; // section-title 会被 Tailwind CSS 处理
const errorMessageClass = "error-message"; // error-message 样式在原 HTML 的 <style> 中定义，需要迁移或用 Tailwind 替代

function handleAstrolabeCalculation(formData) {
  currentAstrolabeData.value = null;
  errorMessage.value = '';
  isLoading.value = true;

  // console.log('Form Data Received:', formData);

  if (!formData.solarDate) {
    errorMessage.value = '請選擇陽曆生日。';
    isLoading.value = false;
    return;
  }

  try {
    // Check if the imported astro object and its bySolar method are available
    if (typeof astro === 'undefined' || typeof astro.bySolar !== 'function') {
      errorMessage.value = 'iztro 庫的 astro 對象或 bySolar 方法未能正確初始化。請檢查 iztro 導入和版本。';
      isLoading.value = false;
      return;
    }
    
    // Correctly call bySolar on the imported astro object
    const data = astro.bySolar(formData.solarDate, formData.timeIndex, formData.gender, true, 'zh-TW');
    
    setTimeout(() => {
        currentAstrolabeData.value = data; 
        isLoading.value = false;
    }, 100); 

  } catch (e) {
    console.error("排盤時發生錯誤:", e);
    errorMessage.value = '排盤時發生錯誤：' + e.message + '。請檢查輸入或查看控制台獲取更多資訊。';
    isLoading.value = false;
  }
}

</script>

<style>
/* 全局样式已在 main.css 或 App.vue 的 <style> 标签中定义 */
body {
  font-family: 'Inter', sans-serif;
  /* background-color: #f3f4f6; Tailwind gray-100 - 已在 app-container 使用 Tailwind 类 */
}
.container {
  max-width: 1200px; 
  margin: 2rem auto;
  padding: 2rem;
  background-color: #ffffff; /* White */
  border-radius: 0.75rem; /* Tailwind rounded-xl */
  box-shadow: 0 10px 15px -3px rgba(0, 0, 0, 0.1), 0 4px 6px -2px rgba(0, 0, 0, 0.05); /* Tailwind shadow-lg */
}

/* 从原HTML复制过来的特定样式，确保它们被应用 */
.section-title {
    font-size: 1.5rem; /* Tailwind text-2xl */
    font-weight: 700; /* Tailwind font-bold */
    margin-top: 2rem;
    margin-bottom: 1rem;
    color: #1f2937; /* Tailwind gray-800 */
    border-bottom: 2px solid #e5e7eb; /* Tailwind gray-200 */
    padding-bottom: 0.5rem;
}

.error-message {
    color: #ef4444; /* Tailwind red-500 */
    background-color: #fee2e2; /* Tailwind red-100 */
    border: 1px solid #fca5a5; /* Tailwind red-300 */
    padding: 0.75rem;
    border-radius: 0.375rem;
    margin-top: 1rem;
    margin-bottom: 1rem; /* Added for spacing */
}

pre.raw-json { 
    background-color: #1f2937; /* Tailwind gray-800 */
    color: #f3f4f6; /* Tailwind gray-100 */
    padding: 1rem;
    border-radius: 0.375rem; 
    overflow-x: auto;
    white-space: pre-wrap;
    word-wrap: break-word;
    font-size: 0.8rem;
    margin-top: 1rem;
}
</style> 