<template>
  <div v-if="astrolabeData">
    <!-- 基本资料 -->
    <h2 class="section-title-vue">基本資料（中宮資訊）</h2>
    <div class="info-grid-vue mb-6">
      <InfoItem title="性別" :value="astrolabeData.gender" />
      <InfoItem title="陽曆生日" :value="astrolabeData.solarDate" />
      <InfoItem title="農曆生日" :value="astrolabeData.lunarDate" />
      <InfoItem title="四柱 (干支)" :value="astrolabeData.chineseDate" />
      <InfoItem title="時辰" :value="`${astrolabeData.time} (${astrolabeData.timeRange})`" />
      <InfoItem title="生肖" :value="astrolabeData.zodiac" />
      <InfoItem title="星座" :value="astrolabeData.sign" />
      <InfoItem title="命主" :value="astrolabeData.soul" />
      <InfoItem title="身主" :value="astrolabeData.body" />
      <InfoItem title="五行局" :value="astrolabeData.fiveElementsClass" />
    </div>

    <!-- 十二宫垣 -->
    <h2 class="section-title-vue">十二宮垣 (4x3 網格)</h2>
    <div class="palace-grid-container-vue">
      <div class="palace-grid-vue">
        <div v-for="palace in orderedPalaces" :key="palace.name" class="palace-card-vue">
          <h3 class="palace-title-vue">
            {{ palace.name }} ({{ palace.heavenlyStem }}{{ palace.earthlyBranch }})
            <span v-if="palace.isBodyPalace" class="text-xs text-red-500">(身)</span>
            <span v-if="palace.isOriginalPalace" class="text-xs text-purple-500">(來因)</span>
          </h3>
          <div>
            <ul class="stars-list-vue" v-if="palace.majorStars && palace.majorStars.length">
              <li v-for="star in palace.majorStars" :key="star.name + '-major'" v-html="formatStarDisplay(star)"></li>
            </ul>
            <ul class="stars-list-vue !text-xs" v-if="palace.minorStars && palace.minorStars.length">
              <li v-for="star in palace.minorStars" :key="star.name + '-minor'" v-html="formatStarDisplay(star)"></li>
            </ul>
            <ul class="stars-list-vue !text-xs" v-if="palace.adjectiveStars && palace.adjectiveStars.length">
              <li v-for="star in palace.adjectiveStars" :key="star.name + '-adj'" v-html="formatStarDisplay(star)"></li>
            </ul>
          </div>
          <div class="palace-details-vue">
            <p>{{ palace.changsheng12 || '-' }}/{{ palace.boshi12 || '-' }}</p>
            <p>{{ palace.jiangqian12 || '-' }}/{{ palace.suiqian12 || '-' }}</p>
            <p>大限: {{ palace.decadal.range.join('-') }}</p>
            <p>小限: {{ palace.ages[0] || '-' }}</p>
          </div>
        </div>
      </div>
    </div>

    <!-- 原始 JSON 数据 -->
    <details class="mt-6">
      <summary class="text-lg font-semibold cursor-pointer text-gray-700">原始星盤 JSON 數據</summary>
      <div class="p-4">
        <pre class="raw-json-vue">{{ JSON.stringify(astrolabeData, null, 2) }}</pre>
      </div>
    </details>

  </div>
</template>

<script setup>
import { computed } from 'vue';
import InfoItem from './InfoItem.vue'; // 子组件，用于显示每一项基本信息

const props = defineProps({
  astrolabeData: {
    type: Object,
    required: true
  }
});

// 与原HTML中相同的宫位顺序
const palaceOrderIndices = [3, 4, 5, 6, 2, 1, 0, 7, 11, 10, 9, 8];

const orderedPalaces = computed(() => {
  if (!props.astrolabeData || !props.astrolabeData.palaces) return [];
  return palaceOrderIndices.map(index => props.astrolabeData.palaces[index]).filter(p => p);
});

function formatStarDisplay(star) {
  if (!star) return '';
  let starDisplay = star.name || '未知星曜';
  if (star.mutagen) {
    starDisplay += ` <span class="mutagen-vue">(${star.mutagen})</span>`;
  }
  if (star.brightness) {
    starDisplay += ` <span class="star-brightness-vue">[${star.brightness}]</span>`;
  }
  return starDisplay; // 使用 v-html 来渲染，因为包含 span 标签
}

</script>

<style scoped>
/* 样式从 App.vue 或原HTML迁移并调整，添加 -vue 后缀以避免与 App.vue 中的全局类冲突，虽然 scoped 通常能处理 */
.section-title-vue {
  font-size: 1.5rem; 
  font-weight: 700; 
  margin-top: 2rem;
  margin-bottom: 1rem;
  color: #1f2937; 
  border-bottom: 2px solid #e5e7eb; 
  padding-bottom: 0.5rem;
}

.info-grid-vue {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(280px, 1fr));
  gap: 1.25rem; 
}

.palace-grid-container-vue { 
  border: 2px solid #60a5fa; 
  border-radius: 0.5rem;
  padding: 1rem;
  background-color: #eff6ff; 
}

.palace-grid-vue {
  display: grid;
  grid-template-columns: repeat(4, minmax(0, 1fr)); 
  gap: 0.75rem; 
}

.palace-card-vue {
  background-color: #ffffff; 
  border: 1px solid #d1d5db; 
  border-radius: 0.375rem; 
  padding: 0.75rem; 
  min-height: 180px; 
  display: flex;
  flex-direction: column;
  justify-content: space-between;
}

.palace-title-vue {
  font-size: 1rem; 
  font-weight: 700; 
  color: #1e3a8a; 
  margin-bottom: 0.5rem;
  text-align: center;
}

.stars-list-vue {
  font-size: 0.75rem; 
  list-style: none; /* Ensure no bullets */
  padding-left: 0; /* Remove default padding */
}

.stars-list-vue li {
  padding: 0.125rem 0.375rem; 
  margin-right: 0.25rem;
  margin-bottom: 0.25rem;
  display: inline-block; /* Or flex for better control if needed */
}

/* These might need to be global if v-html is used and spans are not components */
:deep(.mutagen-vue) {
  /* Style for mutagens, e.g., color: red; */
  color: #ef4444; /* Tailwind red-500 */
  font-weight: bold;
}

:deep(.star-brightness-vue) {
  /* Style for brightness, e.g., color: blue; */
  color: #3b82f6; /* Tailwind blue-500 */
  font-style: italic;
}

.palace-details-vue {
  font-size: 0.7rem; 
  color: #4b5563; 
  margin-top: auto; 
  text-align: center;
}

.palace-details-vue p {
  margin-bottom: 0.1rem;
}

pre.raw-json-vue { 
  background-color: #1f2937; 
  color: #f3f4f6; 
  padding: 1rem;
  border-radius: 0.375rem; 
  overflow-x: auto;
  white-space: pre-wrap;
  word-wrap: break-word;
  font-size: 0.8rem;
}
</style> 