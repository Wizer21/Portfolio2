<template>
  <div>
    <div id="imagine_holder">
      <div id="scene_3d">
      </div>
      <div id="imagine_text">
        <h2>
          Imagine
        </h2>
      </div>    
    </div>
  </div>
</template>

<script>
import * as THREE from 'three';
import oc from 'three-orbit-controls';
import { GLTFLoader } from 'three/examples/jsm/loaders/GLTFLoader';

export default {
  name: 'Imagine',
  data(){
    return {
      lastPos: 0,
      cameraRotation: [-3.14, 0.06, 3.14],
      cameraPosition: [1.22, 0.87, -9.99],
      scrolling: false,
      timer: null,
      houseRotationY: 3,
      houseRotationX: 0.3
    }
  },
  methods: {
    updateScene(data) {
      let pos = data.scroll.y
      if (this.lastPos != 0){
        this.houseRotationY += (this.lastPos - pos) * 0.002
        this.houseRotationX += (this.lastPos - pos) * 0.0003

        this.scrolling = true

        clearTimeout(this.timer)
        this.timer = setTimeout(() => {
          this.scrolling = false
        }, 100)
      }
      this.lastPos = pos
    },
    rotateCamera(camera, house){
      if (this.scrolling){
        // Calculate rotation modification
        let camera_rotation_x = (this.cameraRotation[0] - camera.rotation.x ) * 0.05
        let camera_rotation_y = (this.cameraRotation[1] - camera.rotation.y ) * 0.05
        let camera_rotation_z = (this.cameraRotation[2] - camera.rotation.z ) * 0.05
        // Apply rotation modification
        camera.rotation.x += camera_rotation_x
        camera.rotation.y += camera_rotation_y
        camera.rotation.z += camera_rotation_z
        
        // Calculate position modification
        let camera_position_x = (this.cameraPosition[0] - camera.position.x ) * 0.05
        let camera_position_y = (this.cameraPosition[1] - camera.position.y ) * 0.05
        let camera_position_z = (this.cameraPosition[2] - camera.position.z ) * 0.05
        // Apply position modification        
        camera.position.set(camera.position.x + camera_position_x, camera.position.y + camera_position_y, camera.position.z + camera_position_z)

        // Calculate House rotation modification        
        let house_rotationX = (this.houseRotationX - house.rotation.x ) * 0.05
        let house_rotationY = (this.houseRotationY - house.rotation.y ) * 0.05
        // Apply House rotation modification        
        house.rotation.x += house_rotationX
        house.rotation.y += house_rotationY      
      }
    }
  },
  mounted() {
    // --- 3D SCENE ---
    const OrbitControls = oc(THREE)
    const three_scene = document.getElementById('scene_3d')
    let scene = new THREE.Scene()
    let local = this

    // Load 3D elements
    const loader = new GLTFLoader() 
    let house
    loader.load("model/casa.glb", function ( gltf ) {
      house = scene.add( gltf.scene )    
      house = house.children[house.children.length - 1]
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
    camera.position.set(this.cameraPosition[0], this.cameraPosition[1], this.cameraPosition[2])

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

      if(camera){
        local.rotateCamera(camera, house)        
      }
      
      renderer.render(scene, camera)
    }
    animate()
  }
}
</script>

<style scoped>
#imagine_holder
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
#imagine_text h2
{
  font-size: 15vw;
}
</style>
