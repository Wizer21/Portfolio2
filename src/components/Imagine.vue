<template>
  <div id="imagine">
    <div id="scene_3d">
    </div>
    <div id="imagine_text">
      <h2>
        Imagine
      </h2>
    </div>    
  </div>
</template>

<script>
import * as THREE from 'three';
import oc from 'three-orbit-controls';
import { GLTFLoader } from 'three/examples/jsm/loaders/GLTFLoader';

export default {
  name: 'Imagine',
  mounted() {
    // --- 3D SCENE ---
    const OrbitControls = oc(THREE)
    const three_scene = document.getElementById('scene_3d')

    let scene = new THREE.Scene()

    // Load 3D elements
    const loader = new GLTFLoader() 
    let house
    loader.load("model/casa.glb", function ( gltf ) {
      house = scene.add( gltf.scene )    
      house = house.children[house.children.length - 1]
      house.rotation.y = 1
    }) 

    // Lantern pos = (6.5, 1.2, -4)
    const point_light = new THREE.PointLight( 0xffffff, 1, 100 )
    point_light.position.set(6.5, 1.2, -4)
    scene.add( point_light );


    // Camera
    const camera = new THREE.PerspectiveCamera(
      75,
      window.innerWidth / window.innerHeight,
      0.1,
      100
    )
    camera.position.set(0, 0, -80)

    // Render
    let renderer = new THREE.WebGLRenderer({ alpha: true , antialias: true })
    renderer.setClearColor( 0x000000, 0 )
    three_scene.appendChild(renderer.domElement)

    // Resize
    renderer.setSize(window.innerWidth, window.innerHeight)

    // Camera Controler    
    let controls = new OrbitControls(camera, renderer.domElement)
    controls.minDistance = 2
    controls.maxDistance = 10
    controls.target.set(0, 1, 0)
    controls.enableZoom = false
    controls.update()

    const animate = function () {
      requestAnimationFrame(animate)
      renderer.render(scene, camera)
    }
    animate()
  }
}
</script>

<style scoped>
#imagine
{
  display: grid;
}
#imagine_text
{
  display: flex;
  justify-content: center;
  align-items: center;
  
  width: 100vw;
  height: 100vh;
  grid-column: 1;
  grid-row: 1;

  z-index: 2;
  pointer-events: none;
}
#scene_3d
{
  grid-column: 1;
  grid-row: 1;
}
#imagine h2
{
  font-size: 15vw;
}
</style>
