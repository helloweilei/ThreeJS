<script setup lang="ts">
import * as THREE from "three";
import { OrbitControls } from "three/examples/jsm/controls/OrbitControls";

const scene = new THREE.Scene();

const geo = new THREE.SphereGeometry(1);
const material = new THREE.MeshBasicMaterial({ color: 0xff0000 });
const sun = new THREE.Mesh(geo, material.clone());
const earth = new THREE.Mesh(geo.clone(), material.clone());
const moon = new THREE.Mesh(geo.clone(), material.clone());

earth.scale.set(0.5, 0.5, 0.5);
moon.scale.set(0.2, 0.2, 0.2);
earth.material.color = new THREE.Color(0x0000ff);
moon.material.color = new THREE.Color(0x666666);

const group1 = new THREE.Group();
group1.add(earth, moon);
group1.position.set(3, 0, 0);
moon.position.x = 1;
const group2 = new THREE.Group();
group2.add(sun, group1);

scene.add(group2);

// Camera
const camera = new THREE.PerspectiveCamera(75, innerWidth / innerHeight, 0.1, 100);
camera.position.set(0, 0, 10);
scene.add(camera);

// 渲染器
const renderer = new THREE.WebGLRenderer();
renderer.setSize(innerWidth, innerHeight);
renderer.render(scene, camera);

const orbitControl = new OrbitControls(camera, renderer.domElement);
document.body.append(renderer.domElement);

const clock = new THREE.Clock();
const tick = () => {
  const time = clock.getElapsedTime();
  group2.rotation.z = time;
  group1.rotation.z = time * 0.6;
  renderer.render(scene, camera);
  requestAnimationFrame(tick);
  orbitControl.update();
};
tick();
</script>

<template>
  <div class="container"></div>
</template>

<style scoped>
.container {
  width: 100%;
  height: 100%;
  background-color: blueviolet;
  display: none;
}
</style>
