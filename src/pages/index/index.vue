<script setup>
import {ref, onMounted} from 'vue'
import {
  Scene,
  PerspectiveCamera,
  WebGLRenderer,
  Color,
  AxesHelper,
  PlaneGeometry,
  MeshBasicMaterial,
  Mesh,
  BoxGeometry,
  SphereGeometry, PointLight, AmbientLight,
} from 'three'
import * as THREE from 'three'
import {OrbitControls} from 'three/examples/jsm/controls/OrbitControls'
import {GLTFLoader} from "three/examples/jsm/loaders/GLTFLoader"

const container = ref()
const init = () => {
  // 场景
  const scene = new Scene()

  // 相机
  const camera = new PerspectiveCamera(75, window.innerWidth / window.innerHeight, 0.1, 10000)
  camera.position.set(60, 30, 120);
  camera.lookAt(scene.position); // 看向场景中心
  // 渲染器
  const renderer = new WebGLRenderer()
  renderer.setClearColor(new Color(0xEEEEEE))
  renderer.setSize(window.innerWidth, window.innerHeight)

  // 添加 OrbitControls
  const controls = new OrbitControls(camera, renderer.domElement)
  controls.enableDamping = true
  controls.dampingFactor = 0.25
  controls.screenSpacePanning = false
  controls.maxPolarAngle = Math.PI / 2

  //创建立方体
  const boxgeometry = new THREE.BoxGeometry(20, 20, 20);
  const boxMaterial = new THREE.MeshBasicMaterial({
    color: 0x77777ff,
    wireframe: true
  });
  const box = new THREE.Mesh(boxgeometry, boxMaterial);
  scene.add(box);

  const loader = new GLTFLoader();
  loader.load("./CMX10.gltf", gltf => {
        scene.add(gltf.scene);
        // 设置模型缩放
        const scaleFactor = 0.01; // 调整缩放因子以达到所需的缩放水平
        gltf.scene.scale.set(scaleFactor, scaleFactor, scaleFactor);
        // gltf.scene.Material.wireframe = true
        camera.lookAt(gltf.scene.position);
      },
      function (xhr) {
        console.log((xhr.loaded / xhr.total) * 100 + "% loaded");
      },
      function (error) {
        console.log(error,"An error happened");
      }
  );


  //挂载dom
  container.value.appendChild(renderer.domElement)
  // renderer.render(scene, camera)
  //绘制画布
  function render () {
    requestAnimationFrame(render);
    renderer.render(scene, camera)
  }
  render()

}

onMounted(() => {
  init()
})
</script>

<template>
  <div ref="container"/>
</template>

<style lang="scss" scoped>

</style>
