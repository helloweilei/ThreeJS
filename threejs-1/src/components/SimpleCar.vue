<script setup lang="ts">
import * as THREE from "three";
import { OrbitControls } from "three/examples/jsm/controls/OrbitControls";

const scene = new THREE.Scene();

const CAR_WIDTH = 2;
const CAR_HEIGHT = 4;
const CAR_DEPTH = 1;

const geo = new THREE.BoxGeometry(CAR_WIDTH, CAR_HEIGHT, CAR_DEPTH);
const mat = new THREE.MeshNormalMaterial();
const car = new THREE.Group();
const body = new THREE.Mesh(geo, mat);
const head = new THREE.Mesh(new THREE.BoxGeometry(CAR_WIDTH, CAR_HEIGHT / 2, CAR_DEPTH * 1.5), mat);
head.position.z = CAR_DEPTH / 2;
head.position.y = CAR_HEIGHT / 4;

const createWheel = () => {
    const WHEEL_RADIUS = 0.6;
    const wheel = new THREE.Group();
    // 轮毂
    const wheelHub = new THREE.Mesh(new THREE.BoxGeometry(WHEEL_RADIUS * 2, WHEEL_RADIUS * 2, 0.2), mat);
    // 轮胎
    const tire = new THREE.Group();
    const count = 100;
    const step = Math.PI * 2 / count;
    Array.from({length: count}).forEach((_, index) => {
        const seg = new THREE.Mesh(
            new THREE.BoxGeometry(0.1, 0.1, 0.3),
            new THREE.MeshBasicMaterial({ color: 0x333333 })
        );
        seg.position.set(Math.cos(step * index), Math.sin(step * index), 0);
        seg.rotation.z = step * index;
        tire.add(seg);
    });
    wheel.add(wheelHub);
    wheel.add(tire);
    wheel.rotation.y = Math.PI / 2;
    return wheel;
}
// 轮子
const wheels = Array.from({length: 4}).map(createWheel);
wheels[0].position.set(CAR_WIDTH / 2 + 0.3, CAR_HEIGHT / 2 - 0.6, -0.3);
wheels[1].position.set(CAR_WIDTH / 2 + 0.3, - CAR_HEIGHT / 2 + 0.6, -0.3);
wheels[2].position.set(-CAR_WIDTH / 2 - 0.3, CAR_HEIGHT / 2 - 0.6, -0.3);
wheels[3].position.set(-CAR_WIDTH / 2 - 0.3, - CAR_HEIGHT / 2 + 0.6, -0.3);
car.add(body);
car.add(head);

scene.add(car);
wheels.forEach(wheel => car.add(wheel));

// Camera
const camera = new THREE.PerspectiveCamera(75, innerWidth / innerHeight, 0.1, 100);
camera.position.set(0, 0, 10);
scene.add(camera);

// 渲染器
const renderer = new THREE.WebGLRenderer();
renderer.setSize(innerWidth, innerHeight);
renderer.render(scene, camera);

const orbitControl = new OrbitControls(camera, renderer.domElement);
document.getElementById('app')!.append(renderer.domElement);

const clock = new THREE.Clock();
const tick = () => {
  const time = clock.getElapsedTime();
  wheels.forEach(wheel => wheel.rotation.x = -time);
  car.position.y = time % 6;
  renderer.render(scene, camera);
  requestAnimationFrame(tick);
  orbitControl.update();
};
tick();
</script>

<template>
</template>

