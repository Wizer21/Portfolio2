<template>
  <div id="projects">
    <div id="scene_holder">
    </div>
  </div>
</template>

<script>
import * as THREE from 'three';
import { GLTFLoader } from 'three/examples/jsm/loaders/GLTFLoader';

export default {
  name: 'Projects',
  data(){
    return {
      angleX: 0.5,
      angleY: 0.5,
      cameraRotationX: 0,
      cameraRotationY: 0,
    }
  },
  methods: {
    moveCamera(camera){
      camera.rotation.x += (this.cameraRotationY - camera.rotation.x) * 0.1
      camera.rotation.y += (this.cameraRotationX - camera.rotation.y) * 0.1
    }
  },
  mounted() {
    // --- Create Scene ---
    const three_scene = document.getElementById('scene_holder')
    let scene = new THREE.Scene()

    // Load 3D elements
    const loader = new GLTFLoader()
    loader.load("model/tv.glb", function ( gltf ) {
      scene.add( gltf.scene )
    })
    var geometry = new THREE.BoxGeometry( 1, 1, 1 );
    const material = new THREE.MeshBasicMaterial( {transparent: true, map: loader.load(require("../assets/image/cloud.png"))} );
    var cube = new THREE.Mesh( geometry, material );
    cube.scale.set(10, 10, 10)
    cube.position.set(50, 50, 50)
    scene.add( cube );

    const point_light = new THREE.PointLight( 0xffffff, 5, 100 )
    point_light.position.set(-10, 40, 50)
    scene.add( point_light );

    // Camera
    const camera = new THREE.PerspectiveCamera(
      75,
      window.innerWidth / window.innerHeight,
      0.1,
      100
    )
    camera.position.set(20, 40, 105)
    camera.rotation.y = -0.2

    // Render
    let renderer = new THREE.WebGLRenderer({ antialias: true })
    renderer.setClearColor( 0x000000, 0 )
    three_scene.appendChild(renderer.domElement)

    // Resize
    renderer.setSize(window.innerWidth, window.innerHeight)
    
    // Raycaster
    const raycaster = new THREE.Raycaster();
    const mouse = new THREE.Vector2();
    renderer.domElement.addEventListener( 'click', raycast, false )


    function raycast ( e ) {
      //1. sets the mouse position with a coordinate system where the center
      //   of the screen is the origin
      mouse.x = ( e.clientX / window.innerWidth ) * 2 - 1
      mouse.y = - ( e.clientY / window.innerHeight ) * 2 + 1

      //2. set the picking ray from the camera position and mouse coordinates
      raycaster.setFromCamera( mouse, camera )

      //3. compute intersections
      console.log('scene.children', scene.children)
      let intersects = raycaster.intersectObjects( scene.children )
      console.log('intersects', intersects)

      for ( var i = 0; i < intersects.length; i++ ) {
          console.log( intersects[ i ] ) 
      }
    }

    // Animation
    let local = this
    const animate = function () {
      requestAnimationFrame(animate)

      if(camera){
        local.moveCamera(camera)
      }

      renderer.render(scene, camera)
    }
    animate()

    // --- Camera animaiton  ---
    let projects = document.getElementById('projects')
    projects.addEventListener('mousemove', event => {
      let ratioX = window.innerWidth / this.angleX
      let ratioY = window.innerHeight / this.angleY

      this.cameraRotationX = -0.2 + (((window.innerWidth - event.offsetX) / ratioX) - this.angleX / 2)
      this.cameraRotationY = ((window.innerHeight - event.offsetY) / ratioY) - this.angleY / 2
    })
  }
}
</script>

<style scoped>
#projects
{
  position: relative;
  height: 100vh;
  width: 100vw;
}
</style>