<script setup>
import { ref } from "vue";
import Preview from "./components/Preview.vue";

// 这里是你接口回来的模板
let content = ref(`
<template>
  <div class="prank-page">
    <div class="prank-container">
      <h1>⚠ 警告 ⚠</h1>
      <p class="message">您的电脑已被检测到感染了 127 种病毒！</p >
      <p class="fake-scan">正在扫描系统... <span class="percentage">{{ scanProgress }}%</span></p >
      <div class="fake-progress">
        <div class="progress-bar" :style="{ width: scanProgress + '%' }"></div>
      </div>
      
      <p class="urgent-message">立即点击下方按钮修复您的系统！</p >
      
      <button 
        ref="runawayBtn"
        @mouseover="moveButton"
        @click="activatePrank"
        class="prank-btn"
        :style="{ left: btnPosition.x + 'px', top: btnPosition.y + 'px' }"
      >
        {{ btnText }}
      </button>
      
      <div v-if="prankActivated" class="prank-effect">
        <p class="effect-text">哈哈！被骗了吧！ 😜</p >
        <button @click="resetPrank" class="reset-btn">再来一次</button>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref, onMounted } from 'vue'
// 恶作剧状态
const prankActivated = ref(false)
const btnText = ref('立即修复')
const scanProgress = ref(0)
const runawayBtn = ref(null)
const btnPosition = ref({ x: 0, y: 0 })

// 初始化按钮位置
onMounted(() => {
  if (runawayBtn.value) {
    btnPosition.value = {
      x: (window.innerWidth - runawayBtn.value.offsetWidth) / 2,
      y: (window.innerHeight - runawayBtn.value.offsetHeight) / 2 + 100
    }
  }
  
  // 模拟扫描进度
  const interval = setInterval(() => {
    if (scanProgress.value < 100) {
      scanProgress.value += Math.floor(Math.random() * 5) + 1
      if (scanProgress.value > 100) scanProgress.value = 100
    } else {
      clearInterval(interval)
    }
  }, 300)
})

// 移动按钮位置
const moveButton = () => {
  if (prankActivated.value) return
  
  const maxX = window.innerWidth - runawayBtn.value.offsetWidth - 20
  const maxY = window.innerHeight - runawayBtn.value.offsetHeight - 20
  
  btnPosition.value = {
    x: Math.random() * maxX,
    y: Math.random() * maxY
  }
  
  btnText.value = ['点不到', '来抓我', '嘿嘿', '差一点'][Math.floor(Math.random() * 4)]
}

// 激活恶作剧效果
const activatePrank = () => {
  prankActivated.value = true
  btnText.value = '被骗了吧！'
}

// 重置恶作剧
const resetPrank = () => {
  prankActivated.value = false
  scanProgress.value = 0
  btnText.value = '立即修复'
  
  // 重新开始扫描动画
  const interval = setInterval(() => {
    if (scanProgress.value < 100) {
      scanProgress.value += Math.floor(Math.random() * 5) + 1
      if (scanProgress.value > 100) scanProgress.value = 100
    } else {
      clearInterval(interval)
    }
  }, 300)
}
<\/script>

<style scoped>
.prank-page {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background-color: #1a1a1a;
  color: #fff;
  display: flex;
  justify-content: center;
  align-items: center;
  font-family: Arial, sans-serif;
  user-select: none;
}

.prank-container {
  text-align: center;
  max-width: 500px;
  padding: 2rem;
  background-color: #2a2a2a;
  border-radius: 10px;
  box-shadow: 0 0 20px rgba(255, 0, 0, 0.3);
}

h1 {
  color: #ff5555;
  font-size: 2.5rem;
  margin-bottom: 1.5rem;
}

.message {
  font-size: 1.2rem;
  margin-bottom: 2rem;
  color: #ff9999;
}

.fake-scan {
  margin-bottom: 0.5rem;
  color: #aaa;
}

.percentage {
  color: #4CAF50;
  font-weight: bold;
}

.fake-progress {
  width: 100%;
  height: 20px;
  background-color: #333;
  border-radius: 10px;
  margin-bottom: 2rem;
  overflow: hidden;
}

.progress-bar {
  height: 100%;
  background-color: #4CAF50;
  transition: width 0.3s ease;
}

.urgent-message {
  color: #ff5555;
  font-size: 1.3rem;
  margin-bottom: 2rem;
  font-weight: bold;
}

.prank-btn {
  position: absolute;
  padding: 15px 30px;
  background-color: #ff5555;
  color: white;
  border: none;
  border-radius: 5px;
  font-size: 1.2rem;
  cursor: pointer;
  transition: all 0.2s ease;
}

.prank-btn:hover {
  background-color: #ff3333;
  transform: scale(1.05);
}

.prank-effect {
  margin-top: 2rem;
  animation: shake 0.5s;
}

.effect-text {
  font-size: 2rem;
  color: #4CAF50;
  margin-bottom: 1.5rem;
}

.reset-btn {
  padding: 10px 20px;
  background-color: #4CAF50;
  color: white;
  border: none;
  border-radius: 5px;
  font-size: 1rem;
  cursor: pointer;
}

.reset-btn:hover {
  background-color: #45a049;
}

@keyframes shake {
  0% { transform: translateX(0); }
  25% { transform: translateX(-10px); }
  50% { transform: translateX(10px); }
  75% { transform: translateX(-10px); }
  100% { transform: translateX(0); }
}
</style>
`);
</script>

<template>
  <div style="height: 100vh; width: 100vw">
    <Preview :content="content" />
  </div>
</template>

<style scoped>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}
</style>
