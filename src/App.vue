<template>
  <div>
    <h1>工具集合</h1>

    <!-- 获取当前位置功能 -->
    <div>
      <h2>当前位置</h2>
      <p>纬度: {{ latitude }}</p>
      <p>经度: {{ longitude }}</p>
      <p>时间戳: {{ timestamp }}</p>
      <button @click="getLocation">获取当前位置</button>
    </div>

    <hr />

    <!-- CDP 自动化检测功能 -->
    <div>
      <h2>CDP 自动化检测</h2>
      <textarea v-model="detectionResult" rows="10" cols="50" readonly></textarea>
      <br />
      <button @click="detectCDP">检测 CDP</button>
    </div>
  </div>
</template>

<script lang="ts">
import { ref } from 'vue'

export default {
  setup() {
    // 获取当前位置
    const latitude = ref<number | null>(null)
    const longitude = ref<number | null>(null)
    const timestamp = ref<number | null>(null)

    const getLocation = () => {
      if (navigator.geolocation) {
        navigator.geolocation.getCurrentPosition(
            (position) => {
              latitude.value = position.coords.latitude
              longitude.value = position.coords.longitude
              timestamp.value = position.timestamp
            },
            (error) => {
              console.error('获取位置信息出错: ', error.message)
            }
        )
      } else {
        console.error('浏览器不支持Geolocation')
      }
    }

    // CDP 自动化检测
    const detectionResult = ref<string>('')

    const detectCDP = () => {
      let detected = false
      const e = new Error()

      // 定义栈属性的 getter
      Object.defineProperty(e, 'stack', {
        get() {
          detected = true
        }
      })

      // 触发 CDP 序列化
      console.log(e)

      // 检测结果
      if (detected) {
        detectionResult.value = '浏览器处于自动化控制中（可能使用了 CDP）。'
      } else {
        detectionResult.value = '未检测到自动化控制，浏览器应该是手动操作的。'
      }
    }

    return {
      latitude,
      longitude,
      timestamp,
      getLocation,
      detectionResult,
      detectCDP
    }
  }
}
</script>

<style scoped>
h1 {
  text-align: center;
  margin-bottom: 20px;
}
h2 {
  color: #333;
}
textarea {
  width: 100%;
  resize: none;
}
button {
  margin-top: 10px;
  padding: 10px 20px;
  background-color: #42b983;
  color: white;
  border: none;
  border-radius: 5px;
  cursor: pointer;
}
button:hover {
  background-color: #369f6b;
}
hr {
  margin: 20px 0;
}
</style>
