<script setup lang="ts">
import * as THREE from "three";
import { OrbitControls } from "three/examples/jsm/controls/OrbitControls";

const scene = new THREE.Scene();

// Camera
const camera = new THREE.PerspectiveCamera(75, innerWidth / innerHeight, 0.1, 100);
camera.position.set(0, 0, 10);
scene.add(camera);

const car = new THREE.Group();
const frontWheels = new THREE.Group();
const mat = new THREE.MeshNormalMaterial();
const leftWheel = new THREE.Mesh(new THREE.CylinderGeometry(), mat);
frontWheels.add(leftWheel);
car.add(frontWheels);
scene.add(car);

// 渲染器
const renderer = new THREE.WebGLRenderer();
renderer.setSize(innerWidth, innerHeight);
renderer.render(scene, camera);

const orbitControl = new OrbitControls(camera, renderer.domElement);
document.getElementById('app')!.append(renderer.domElement);

const clock = new THREE.Clock();
const tick = () => {
  const time = clock.getElapsedTime();
  renderer.render(scene, camera);
  requestAnimationFrame(tick);
  orbitControl.update();
};
tick();
</script>

<template>
</template>

