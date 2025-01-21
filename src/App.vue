<script setup>
import { ref, onMounted } from 'vue'
import clicksData from './data/clicks.json'
import sitesData from './data/sites.json'

const sites = ref(sitesData)

const clicks = ref(clicksData)

// 根据优先级和点击量排序网站
const sortSitesByClicks = () => {
  sites.value.sort((a, b) => {
    // 如果两个网站优先级不同，优先级高的排在前面
    if (a.priority !== b.priority) {
      return b.priority - a.priority
    }
    // 如果优先级相同，按点击次数排序
    return clicks.value[b.url] - clicks.value[a.url]
  })
}

// 节流函数
const throttle = (fn, delay) => {
  let timer = null
  return function (...args) {
    if (!timer) {
      timer = setTimeout(() => {
        fn.apply(this, args)
        timer = null
      }, delay)
    }
  }
}

// 同步点击数据到文件
const syncClicksToFile = async (clicksData) => {
  try {
    const response = await fetch('/api/updateClicks', {
      method: 'POST',
      headers: {
        'Content-Type': 'application/json',
      },
      body: JSON.stringify(clicksData)
    })
    if (!response.ok) {
      console.error('Failed to sync clicks data')
    }
  } catch (error) {
    console.error('Error syncing clicks data:', error)
  }
}

// 使用节流的同步函数
const throttledSync = throttle(syncClicksToFile, 5000)

// 处理点击事件
const handleClick = (url) => {
  // 如果是新网站，初始化点击次数为1，否则递增
  if (clicks.value[url] === undefined) {
    clicks.value[url] = 1
  } else {
    clicks.value[url]++
  }
  // 将更新后的点击数据写入本地存储
  localStorage.setItem('siteClicks', JSON.stringify(clicks.value))
  // 节流同步到文件
  throttledSync(clicks.value)
  sortSitesByClicks()
}

onMounted(() => {
  // 从本地存储加载点击数据
  const savedClicks = localStorage.getItem('siteClicks')
  if (savedClicks) {
    clicks.value = JSON.parse(savedClicks)
  }
  sortSitesByClicks()
})
</script>

<template>
  <div class="container">
    <h1>我的导航</h1>
    <div class="site-grid">
      <a v-for="site in sites" 
         :key="site.url" 
         :href="site.url" 
         target="_blank"
         class="site-card"
         @click="handleClick(site.url)">
        <img :src="site.icon" :alt="site.name" class="site-icon" />
        <span class="site-name">{{ site.name }}</span>
      </a>
    </div>
  </div>
</template>

<style scoped>
.container {
  max-width: 100%;
  margin: 0 auto;
  padding: 2rem;
}

h1 {
  text-align: center;
  color: #2c3e50;
  margin-bottom: 2rem;
}

.site-grid {
  display: flex;
  flex-wrap: wrap;
  justify-content: center;
  padding: 1rem;
  gap: 1.5rem;
  max-width: 1200px;
  margin: 0 auto;
}

.site-grid::-webkit-scrollbar {
  height: 8px;
}

.site-grid::-webkit-scrollbar-track {
  background: #cbd5e0;
  border-radius: 4px;
}

.site-grid::-webkit-scrollbar-thumb {
  background-color: #4a5568;
  border-radius: 4px;
}

.site-card {
  flex: 0 0 calc(20% - 1.2rem);
  display: flex;
  flex-direction: column;
  align-items: center;
  padding: 1.5rem;
  background: #ffffff;
  border-radius: 8px;
  box-shadow: 0 2px 12px rgba(0, 0, 0, 0.1);
  transition: transform 0.3s ease;
  text-decoration: none;
  color: inherit;
}

@media (max-width: 1200px) {
  .site-card {
    flex: 0 0 calc(25% - 1.2rem);
  }
}

@media (max-width: 992px) {
  .site-card {
    flex: 0 0 calc(33.333% - 1.2rem);
  }
}

@media (max-width: 768px) {
  .site-card {
    flex: 0 0 calc(50% - 1.2rem);
  }
}

@media (max-width: 480px) {
  .site-card {
    flex: 0 0 calc(100% - 1.2rem);
  }
  .site-grid {
    gap: 1rem;
  }
  .container {
    padding: 1rem;
  }
}

.site-card:hover {
  transform: translateY(-5px);
  box-shadow: 0 4px 20px rgba(0, 0, 0, 0.15);
}

.site-icon {
  width: 48px;
  height: 48px;
  margin-bottom: 1rem;
  border-radius: 8px;
}

.site-name {
  font-size: 1rem;
  font-weight: 500;
  color: #2c3e50;
}
</style>
