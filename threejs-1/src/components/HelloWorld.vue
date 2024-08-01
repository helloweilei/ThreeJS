<script setup lang="ts">
import * as THREE from 'three'
import { onMounted, ref } from 'vue';

import Stat from 'three/examples/jsm/libs/stats.module';
import { OrbitControls } from 'three/examples/jsm/controls/OrbitControls';

const stat = new Stat();

// 场景
const scene = new THREE.Scene();
const axes = new THREE.AxesHelper(6);
scene.add(axes);

// 物体：Geometry + Material
const cube = new THREE.BoxGeometry(1, 1, 1);
// const material = new THREE.MeshBasicMaterial();
const material = new THREE.MeshNormalMaterial();
const mesh = new THREE.Mesh(cube, material);
mesh.position.set(1, 1, 1);
// mesh.scale.set(2, 2, 2);
mesh.rotation.x = 45 / 180 * Math.PI;

scene.add(mesh);

// 光源
const light = new THREE.AmbientLight();
scene.add(light);

// Camera
const camera = new THREE.PerspectiveCamera(75, innerWidth / innerHeight, 0.1, 100);
camera.position.set(0, 0, 10);
scene.add(camera);

// 渲染器
const renderer = new THREE.WebGLRenderer();
renderer.setSize(innerWidth, innerHeight);
renderer.render(scene, camera);

const orbitControl = new OrbitControls(camera, renderer.domElement);
const container = ref<HTMLDivElement | null>(null);
document.body.append(renderer.domElement)
document.body.append(stat.dom);
onMounted(() => {
  container.value?.appendChild(renderer.domElement);
  const clock = new THREE.Clock();
  const tick = () => {
    const time = clock.getElapsedTime();
    //mesh.position.x = Math.sin(time) * 2;
    //mesh.position.y = Math.cos(time) * 2;
    mesh.rotation.z = time;
    renderer.render(scene, camera);
    requestAnimationFrame(tick);
    stat.update();
    orbitControl.update();
  }
  tick();
})

</script>

<template>
  <div ref="container" class="container">
  </div>
</template>

<style scoped>
.container {
  width: 100%;
  height: 100%;
  background-color: blueviolet;
}
</style>
