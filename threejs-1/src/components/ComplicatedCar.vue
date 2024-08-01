<script setup lang="ts">
import * as THREE from "three";
import { OrbitControls } from "three/examples/jsm/controls/OrbitControls";

const scene = new THREE.Scene();
const WHEEL_RADIUS = 1.6;

// Camera
const camera = new THREE.PerspectiveCamera(75, innerWidth / innerHeight, 0.1, 100);
camera.position.set(0, 0, 10);
scene.add(camera);

const car = new THREE.Group();
const frontWheels = new THREE.Group();
const mat = new THREE.MeshNormalMaterial();
const leftWheel = new THREE.Group();
let NUM = 5;
for (let i = 0; i < NUM; i++) {
    const m = new THREE.Mesh(new THREE.CylinderGeometry(0.04, 0.04, WHEEL_RADIUS), mat);
    m.rotation.z = Math.PI * 2 / NUM * i;
    leftWheel.add(m);
}
leftWheel.add(new THREE.Mesh(new THREE.TorusGeometry(WHEEL_RADIUS / 2, 0.1), mat));
const bar = new THREE.Mesh(new THREE.CylinderGeometry(0.08, 0.08, 4), mat);
bar.rotation.x = Math.PI / 2;
const rightWheel = leftWheel.clone();
leftWheel.position.z = -2;
rightWheel.position.z = 2;

frontWheels.add(leftWheel);
frontWheels.add(bar);
frontWheels.add(rightWheel);

const backWheels = frontWheels.clone();
backWheels.position.x = 2;
frontWheels.position.x = -2;
const body = new THREE.Mesh(new THREE.BoxGeometry(8, 1, 3.4), mat);
car.add(frontWheels);
car.add(backWheels);
car.add(body);
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
  frontWheels.rotation.z = time;
  backWheels.rotation.z = time;
  renderer.render(scene, camera);
  requestAnimationFrame(tick);
  orbitControl.update();
};
tick();
</script>

<template>
</template>

