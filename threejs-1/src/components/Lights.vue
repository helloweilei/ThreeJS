<script setup lang="ts">
import * as THREE from 'three'
import { OrbitControls } from 'three/examples/jsm/controls/OrbitControls'
import { GUI } from 'dat.gui'

const gui = new GUI()

const scene = new THREE.Scene()
const material = new THREE.MeshPhongMaterial({ color: 0xff0000 })
const sphereGeo = new THREE.SphereGeometry(0.5)
const planeGeo = new THREE.PlaneGeometry(4, 4)
const sphere = new THREE.Mesh(sphereGeo, material)
const plane = new THREE.Mesh(planeGeo, material.clone())
plane.rotation.x = -0.5 * Math.PI
sphere.position.y = 0.5
plane.material.color.set(0xcccccc)

const aLigth = new THREE.AmbientLight(0xffffff, 0.3)
scene.add(aLigth)
const spotLight = new THREE.SpotLight(0xffffff, 50, 3, Math.PI / 4)
spotLight.position.set(2, 2, 0)
const spotLightHelper = new THREE.SpotLightHelper(spotLight)
scene.add(spotLight, spotLightHelper)

const spotControl = {
    color: '#ff0000',
}
const spotFolder = gui.addFolder("Spot Light Color")
spotFolder.addColor(spotControl, 'color').onChange(() => {
    spotLight.color.set(spotControl.color)
})
spotFolder.add(spotLight, 'intensity', 0, 100).name('Intensity')

scene.add(sphere, plane)
const camera = new THREE.PerspectiveCamera(75, innerWidth / innerHeight, 0.1, 100)
camera.position.set(0, 3, 5)
scene.add(camera)

const renderer = new THREE.WebGLRenderer()
renderer.setSize(innerWidth, innerHeight)
renderer.render(scene, camera)
const orbitControl = new OrbitControls(camera, renderer.domElement)
document.getElementById('app')!.appendChild(renderer.domElement)

const clock = new THREE.Clock()
const tick = () => {
    const time = clock.getElapsedTime()
    sphere.position.y = 1.5 + Math.sin(time)
    renderer.render(scene, camera)
    orbitControl.update()
    requestAnimationFrame(tick)
}

tick()
</script>
<template></template>
