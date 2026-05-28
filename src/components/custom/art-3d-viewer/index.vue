<template>
  <div ref="viewerContainer" class="art-3d-viewer">
    <canvas ref="canvasRef" class="viewer-canvas"></canvas>
  </div>
</template>

<script setup lang="ts">
  import { ref, onMounted, onBeforeUnmount } from 'vue'
  import * as THREE from 'three'

  defineOptions({ name: 'Art3DViewer' })

  const viewerContainer = ref<HTMLDivElement>()
  const canvasRef = ref<HTMLCanvasElement>()

  let scene: THREE.Scene
  let camera: THREE.PerspectiveCamera
  let renderer: THREE.WebGLRenderer
  let mainGroup: THREE.Group
  let particles: THREE.Points
  let animationId: number

  const createParticles = (): THREE.Points => {
    const count = 120
    const geometry = new THREE.BufferGeometry()
    const positions = new Float32Array(count * 3)

    for (let i = 0; i < count; i++) {
      positions[i * 3] = (Math.random() - 0.5) * 6
      positions[i * 3 + 1] = (Math.random() - 0.5) * 4
      positions[i * 3 + 2] = (Math.random() - 0.5) * 4
    }

    geometry.setAttribute('position', new THREE.BufferAttribute(positions, 3))

    const material = new THREE.PointsMaterial({
      color: 0x409eff,
      size: 0.015,
      transparent: true,
      opacity: 0.5,
      blending: THREE.AdditiveBlending,
      depthWrite: false
    })

    return new THREE.Points(geometry, material)
  }

  const createDevice = (): THREE.Group => {
    const group = new THREE.Group()

    // Simple sphere body
    const bodyGeo = new THREE.SphereGeometry(0.45, 48, 48)
    const bodyMat = new THREE.MeshStandardMaterial({
      color: 0x3a5a8c,
      roughness: 0.35,
      metalness: 0.5
    })
    group.add(new THREE.Mesh(bodyGeo, bodyMat))

    // Accent ring
    const ringGeo = new THREE.TorusGeometry(0.5, 0.025, 16, 64)
    const ringMat = new THREE.MeshStandardMaterial({
      color: 0x409eff,
      roughness: 0.2,
      metalness: 0.8,
      emissive: 0x409eff,
      emissiveIntensity: 0.4
    })
    const ring = new THREE.Mesh(ringGeo, ringMat)
    ring.rotation.x = Math.PI / 2
    group.add(ring)

    // Inner core
    const coreGeo = new THREE.SphereGeometry(0.15, 32, 32)
    const coreMat = new THREE.MeshStandardMaterial({
      color: 0x67c23a,
      roughness: 0.2,
      metalness: 0.3,
      emissive: 0x67c23a,
      emissiveIntensity: 0.6
    })
    group.add(new THREE.Mesh(coreGeo, coreMat))

    return group
  }

  const initScene = (): void => {
    if (!canvasRef.value || !viewerContainer.value) return

    const container = viewerContainer.value
    const width = container.clientWidth
    const height = container.clientHeight

    scene = new THREE.Scene()

    camera = new THREE.PerspectiveCamera(40, width / height, 0.1, 20)
    camera.position.set(1.5, 0.5, 2.5)
    camera.lookAt(0, 0, 0)

    renderer = new THREE.WebGLRenderer({
      canvas: canvasRef.value,
      antialias: true,
      alpha: true
    })
    renderer.setSize(width, height)
    renderer.setPixelRatio(Math.min(window.devicePixelRatio, 2))

    // Soft lighting
    scene.add(new THREE.AmbientLight(0x8899bb, 2))
    const dirLight = new THREE.DirectionalLight(0xffffff, 1.8)
    dirLight.position.set(3, 3, 4)
    scene.add(dirLight)
    const backLight = new THREE.DirectionalLight(0x409eff, 0.8)
    backLight.position.set(-2, 0, -2)
    scene.add(backLight)

    mainGroup = createDevice()
    scene.add(mainGroup)

    particles = createParticles()
    scene.add(particles)
  }

  const animate = (): void => {
    animationId = requestAnimationFrame(animate)

    // Subtle slow rotation
    mainGroup.rotation.y += 0.004
    mainGroup.rotation.x = Math.sin(Date.now() * 0.0005) * 0.08

    // Gentle particle drift
    particles.rotation.y += 0.001
    particles.rotation.x += 0.0005

    renderer.render(scene, camera)
  }

  const handleResize = (): void => {
    if (!viewerContainer.value) return
    const w = viewerContainer.value.clientWidth
    const h = viewerContainer.value.clientHeight
    camera.aspect = w / h
    camera.updateProjectionMatrix()
    renderer.setSize(w, h)
  }

  onMounted(() => {
    initScene()
    animate()
    window.addEventListener('resize', handleResize)
  })

  onBeforeUnmount(() => {
    cancelAnimationFrame(animationId)
    window.removeEventListener('resize', handleResize)
    scene.traverse((o) => {
      if (o instanceof THREE.Mesh) {
        o.geometry.dispose()
        if (Array.isArray(o.material)) o.material.forEach((m) => m.dispose())
        else o.material.dispose()
      }
    })
    renderer.dispose()
  })
</script>

<style lang="scss" scoped>
  .art-3d-viewer {
    width: 100%;
    height: 100%;
    overflow: hidden;
    border-radius: 8px;

    .viewer-canvas {
      display: block;
      width: 100%;
      height: 100%;
    }
  }
</style>
