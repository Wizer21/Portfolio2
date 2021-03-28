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
      projects: require('../assets/data.json'),
      loadedProject: 0,
      screen_material: null
    }
  },
  methods: {
    moveCamera(camera){
      camera.rotation.x += (this.cameraRotationY - camera.rotation.x) * 0.1
      camera.rotation.y += (this.cameraRotationX - camera.rotation.y) * 0.1
    },
    displayProjet(){
      const loaderTexture = new THREE.TextureLoader()      
      this.screen_material.map = loaderTexture.load(require("../assets/image/black.jpg"))
      setTimeout(()=> {
        this.screen_material.map = loaderTexture.load(require(`../assets/image/projects_images/${this.projects[this.loadedProject].image}`))
      }, 100)
    }
  },
  mounted() {
    // --- Create Scene ---
    const three_scene = document.getElementById('scene_holder')
    let scene = new THREE.Scene()
    let local = this

    // -- Load 3D elements --
    // Tv
    const loader = new GLTFLoader()
    loader.load("model/tv_empty.glb", function ( gltf ) {
      scene.add( gltf.scene )
    })

    // Screen
    let geometry = new THREE.BoxGeometry( 85, 70, 1 )
    this.screen_material = new THREE.MeshStandardMaterial();
    let screen = new THREE.Mesh( geometry, this.screen_material )
    screen.name = "screen"
    screen.position.set(-11.7, 38.5, 29)
    scene.add(screen);
    this.displayProjet()

    // Controller
    let ctrlX = 30
    let ctrlY = 30
    let ctrlZ = 100

    geometry = new THREE.BoxGeometry( 10, 2, 20 )
    let material = new THREE.MeshStandardMaterial({ color: 0x1f1f1f });
    let controller = new THREE.Mesh( geometry, material )
    controller.name = "controller"
    controller.position.set(ctrlX, ctrlY, ctrlZ)
    scene.add(controller);

    const loaderTexture = new THREE.TextureLoader();
    // power
    geometry = new THREE.CylinderGeometry( 1, 1, 2, 20 )
    material = new THREE.MeshBasicMaterial({ transparent: true, map: loaderTexture.load(require("../assets/image/controller/power.jpg"))})
    let power = new THREE.Mesh( geometry, material )

    power.name = "power"
    power.position.set(ctrlX - 3, ctrlY + 0.2, ctrlZ - 8)
    power.rotation.y += 1.5
    scene.add(power)

    // git
    geometry = new THREE.BoxGeometry( 3, 2, 1.5 )
    material = new THREE.MeshBasicMaterial({ map: loaderTexture.load(require("../assets/image/controller/git.jpg"))})
    let git = new THREE.Mesh( geometry, material )
    git.name = "git"
    git.position.set(ctrlX - 2, ctrlY + 0.2, ctrlZ - 5)
    scene.add(git);   

    // visit
    geometry = new THREE.BoxGeometry( 3, 2, 1.5 )
    material = new THREE.MeshBasicMaterial({ map: loaderTexture.load(require("../assets/image/controller/visit.jpg"))})
    let visit = new THREE.Mesh( geometry, material )
    visit.name = "visit"
    visit.position.set(ctrlX + 2, ctrlY + 0.2, ctrlZ - 5)
    scene.add(visit)

    // previous
    geometry = new THREE.CylinderGeometry( 1.5, 1.5, 2, 30 )
    material = new THREE.MeshBasicMaterial({ map: loaderTexture.load(require("../assets/image/controller/left.jpg"))})
    let previous = new THREE.Mesh( geometry, material )
    previous.name = "previous"
    previous.position.set(ctrlX - 2, ctrlY + 0.2, ctrlZ - 2)
    previous.rotation.y += 1.5
    scene.add(previous)

    // next
    geometry = new THREE.CylinderGeometry( 1.5, 1.5, 2, 30 )
    material = new THREE.MeshBasicMaterial({ map: loaderTexture.load(require("../assets/image/controller/right.jpg"))})
    let next = new THREE.Mesh( geometry, material )
    next.name = "next"
    next.position.set(ctrlX + 2, ctrlY + 0.2, ctrlZ - 2)
    next.rotation.y += 1.5
    scene.add(next)

    // Light
    const redPoint_light = new THREE.PointLight( 0xff1100, 0, 100 )
    redPoint_light.position.set(ctrlX, ctrlY, ctrlZ - 12)
    scene.add( redPoint_light )

    const point_light = new THREE.PointLight( 0xffffff, 3, 100 )
    point_light.position.set(-1, 40, 60)
    scene.add( point_light )

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
      mouse.x = ( e.clientX / window.innerWidth ) * 2 - 1
      mouse.y = - ( e.clientY / window.innerHeight ) * 2 + 1

      raycaster.setFromCamera( mouse, camera )

      let intersects = raycaster.intersectObjects( scene.children )
      
      let item_name = intersects[0].object.name
      console.log("clicked on ", item_name ) 

      // Controller connection
      if (item_name == "previous"){
        flash()
        local.loadedProject -= 1
        if (local.loadedProject < 0){
          local.loadedProject = local.projects.length - 1
        }      
        local.displayProjet()  
      }
      else if (item_name == "next"){
        flash()
        local.loadedProject += 1
        if (local.loadedProject > local.projects.length - 1){
          local.loadedProject = 0
        }      
        local.displayProjet()  
      }
      else if (item_name == "visit"){
        flash()
        if (local.projects[local.loadedProject].page_link){
          window.open(local.projects[local.loadedProject].page_link)
        }
      }
      else if (item_name == "git"){
        flash()
        if (local.projects[local.loadedProject].git_link){
          window.open(local.projects[local.loadedProject].git_link)
        }
      }
      else if (item_name == "power"){
        flash()
        local.$emit('exit')
      }

      // Red Light flashing
      function flash (){
        redPoint_light.intensity = 5
        setTimeout(() => {        
          redPoint_light.intensity = 0
        }, 100)
      }
    }

    // Animation
    const animate = function () {
      requestAnimationFrame(animate)

      if(camera){
        local.moveCamera(camera)
      }

      renderer.render(scene, camera)
    }
    animate()

    // --- Camera animation  ---
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