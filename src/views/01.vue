<template>
<div class="container" ref="containerRef"></div>
</template>
<script setup>
import { onMounted, ref } from 'vue'
import * as THREE from 'three'
import { OrbitControls } from 'three/examples/jsm/controls/OrbitControls.js'
const scene = new THREE.Scene()
const camera = new THREE.PerspectiveCamera(75, window.innerWidth / window.innerHeight, 0.1, 1000)
const renderer = new THREE.WebGLRenderer()
const containerRef = ref()
// 创建粒子系统
const createParticles = () => {
  const particlesGeometry = new THREE.BufferGeometry()
  const particleCount = 5000

  const posArray = new Float32Array(particleCount * 3)
  
  for (let i = 0; i < particleCount * 3; i++) {
    posArray[i] = (Math.random() - 0.5) * 5
  }

  particlesGeometry.setAttribute('position', new THREE.BufferAttribute(posArray, 3))

  const particlesMaterial = new THREE.PointsMaterial({
    size: 0.005,
    color: 0xffffff
  })

  const particlesMesh = new THREE.Points(particlesGeometry, particlesMaterial)
  scene.add(particlesMesh)
  
  return particlesMesh
}

onMounted(() => {
  // 设置渲染器
  renderer.setSize(window.innerWidth, window.innerHeight)
  renderer.setPixelRatio(window.devicePixelRatio)
  containerRef.value.appendChild(renderer.domElement)

  // 设置相机位置
  camera.position.z = 3

    // 初始化轨道控制器
  const controls = new OrbitControls(camera, renderer.domElement)
  controls.enableDamping = true // 启用阻尼
  controls.dampingFactor = 0.05 // 阻尼系数
  controls.enableZoom = true // 启用缩放
  controls.enableRotate = true // 启用旋转
  controls.enablePan = true // 启用平移
  controls.autoRotate = false // 自动旋转（可选）
  controls.autoRotateSpeed = 2.0 // 自动旋转速度

  // 创建粒子
  const particles = createParticles()

  // 动画循环
  const animate = () => {
    requestAnimationFrame(animate)

    // 更新控制器
    controls.update()
    
    // 旋转粒子
    particles.rotation.x += 0.001
    particles.rotation.y += 0.001

    renderer.render(scene, camera)
  }
  
  animate()

  // 响应窗口大小变化
  window.addEventListener('resize', () => {
    camera.aspect = window.innerWidth / window.innerHeight
    camera.updateProjectionMatrix()
    renderer.setSize(window.innerWidth, window.innerHeight)
  })
})
</script>
<style scoped>
.container {
    height: 100vh;
    width: 100vw;
}
</style>