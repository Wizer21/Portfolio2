<template>
  <div id="projects">
    <div id="message">
      <p>
        Remote control
      </p>    
      <div id="arrow">
        <img :src="require(`../assets/image/chevron-down.png`)" :alt="icon" >
      </div>
    </div>
    <div id="holder">
      <div id="scene_holder">
      </div>
      <div id="description_holder">
        <div id="project_description">
          <p id="description_title">
            {{ currentProject.title }}
          </p>      
          <div id="description_pannel">
            <p>{{ currentProject.description }}</p>
            <p>{{ currentProject.start_date }} -> {{ currentProject.end_date }}</p>
            <div id="icon_stack_holder">
              <div class="techno_holder" v-for="icon of currentProject.tech" :key="icon">                
                <div class="icon_holder">
                  <img :src="require(`../assets/image/icons/${icon}.svg`)" :alt="icon" >
                </div>
                <p>
                  {{ icon }}
                </p>
              </div>
            </div>   
          </div> 
          <div id="arrow_holder">
            <img :src="require('../assets/image/chevron-up.png')" alt="arrow" @click="togglePanel" id="arrow_img">
          </div>
        </div>
      </div>
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
      screenOffset: -0.2,
      cameraRotationX: 3,
      cameraRotationY: -1,
      projects: require('../assets/data.json').reverse(),
      loadedProject: 0,
      screen_material: null,
      currentProject: null,
      visit: null,
      visitUp: true,
      blockMouseControl: true,
      run: false,
      animate: null,
      panelOpen: true,
      usingTouchScreen: true
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
      
      this.currentProject = this.projects[this.loadedProject]

      if(this.projects[this.loadedProject].page_link && !this.visitUp){
        this.visitUp = true
        for (let i = 0; i < 10; i++){
          setTimeout(() => {
            this.visit.position.y += 0.1
          }, 30 + i*30)
        }
      }
      else if(!this.projects[this.loadedProject].page_link && this.visitUp){
        this.visitUp = false
        for (let i = 0; i < 10; i++){
          setTimeout(() => {
            this.visit.position.y -= 0.1
          }, 30 + i*30)
        }
      }
    },
    lookAway(){
      document.getElementById('description_holder').style.opacity = 0
      this.blockMouseControl = true
      for (let i = 0; i < 30 ; i++ ){
        setTimeout(() => {          
          this.cameraRotationY += (-1 - this.cameraRotationY) * 0.1
          this.cameraRotationX += (3 - this.cameraRotationX) * 0.1
        }, i * 66.66)
      }   
      setTimeout(() => {
        this.run = false
      }, 2000)   
    }, 
    lookTv(){
      this.blockMouseControl = true
      this.run = true
      this.animate()

      for (let i = 0; i < 30 ; i++ ){
        setTimeout(() => {          
          this.cameraRotationY += (0 - this.cameraRotationY) * 0.1
          this.cameraRotationX += (this.screenOffset - this.cameraRotationX) * 0.1
        }, i * 66.66)
      }      
      setTimeout(() => {
        document.getElementById('description_holder').style.opacity = 1
        this.blockMouseControl = false        
      }, 2000)

      // Set Ini animation
      let message = document.getElementById('message')
      message.style.display = "block"
      message.style.animation = ""
      setTimeout(() => {
        message.style.animation = `${this.$style["slide"]} 4s linear forwards`

        setTimeout(() => {
          message.style.display = "none"
        }, 8000)
      }, 1000)
    },
    togglePanel(){
      this.checkTouchScreen()
      
      let description_pannel = document.getElementById('description_pannel')
      let arrow_img = document.getElementById('arrow_img')
      if(this.panelOpen){
        this.panelOpen = false
        description_pannel.style.transform = "translateY(-20%)"
        description_pannel.style.opacity = "0"
        description_pannel.style.position = "absolute"

        arrow_img.src = require('../assets/image/chevron-down.png')
      }
      else{
        this.panelOpen = true
        description_pannel.style.transform = "translateY(0vh)"
        description_pannel.style.opacity = "1"
        description_pannel.style.position = "relative"

        arrow_img.src = require('../assets/image/chevron-up.png')
      }
    },    
    checkTouchScreen(){
      if (this.usingTouchScreen){
        this.blockMouseControl = true

        setTimeout(() => {
          this.blockMouseControl = false
        }, 50)
      }
    },
    setEvents(){
      setTimeout(() => {
        let techno_holder = document.getElementsByClassName('techno_holder')
        for (let holder of techno_holder){
          holder.addEventListener('mouseenter', () => {
            this.blockMouseControl = true
            holder.children[1].style.transform = "translateY(2em)"
            holder.children[1].style.opacity = "1"
          }) 
          holder.addEventListener('mouseleave', () => {
            this.blockMouseControl = false
            holder.children[1].style.transform = "translateY(0em)"
            holder.children[1].style.opacity = "0"
          })
        }
      }, 50)
    }
  },
  beforeMount(){
    this.loadedProject = this.projects.length - 1
    this.currentProject = this.projects[this.loadedProject]
  },
  mounted() {
    // --- Create Scene ---
    const three_scene = document.getElementById('scene_holder')
    let scene = new THREE.Scene()
    let local = this

    // Camera
    const camera = new THREE.PerspectiveCamera(
      75,
      window.innerWidth / window.innerHeight,
      0.1,
      100
    )
    camera.position.set(20, 40, 105)
    camera.rotation.x = this.cameraRotationY
    camera.rotation.y = this.cameraRotationX

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

    if (window.innerWidth < 1000){
      ctrlX = 20
      camera.position.set(20, 40, 110)
      this.screenOffset = 0
      this.angleY = 0.8
      this.angleX = 0.8
    }

    geometry = new THREE.BoxGeometry( 10, 2, 20 )
    let material = new THREE.MeshStandardMaterial({ color: 0x1f1f1f });
    let controller = new THREE.Mesh( geometry, material )
    controller.position.set(ctrlX, ctrlY, ctrlZ)
    scene.add(controller)

    const loaderTexture = new THREE.TextureLoader();
    // power
    geometry = new THREE.CylinderGeometry( 1, 1, 2, 20 )
    material = new THREE.MeshStandardMaterial({ map: loaderTexture.load(require("../assets/image/controller/power.jpg"))})
    let power = new THREE.Mesh( geometry, material )

    power.name = "power"
    power.position.set(ctrlX - 3, ctrlY + 0.2, ctrlZ - 8)
    power.rotation.y += 1.5
    scene.add(power)

    // git
    geometry = new THREE.BoxGeometry( 3, 2, 1.5 )
    material = new THREE.MeshStandardMaterial({ map: loaderTexture.load(require("../assets/image/controller/git.jpg"))})
    let git = new THREE.Mesh( geometry, material )
    git.name = "git"
    git.position.set(ctrlX - 2, ctrlY + 0.2, ctrlZ - 5)
    scene.add(git);   

    // visit
    geometry = new THREE.BoxGeometry( 3, 2, 1.5 )
    material = new THREE.MeshStandardMaterial({ map: loaderTexture.load(require("../assets/image/controller/visit.jpg"))})
    let visit = new THREE.Mesh( geometry, material )
    visit.name = "visit"
    visit.position.set(ctrlX + 2, ctrlY + 0.2, ctrlZ - 5)
    scene.add(visit)
    this.visit = visit

    // previous
    geometry = new THREE.CylinderGeometry( 1.5, 1.5, 2, 30 )
    material = new THREE.MeshStandardMaterial({ map: loaderTexture.load(require("../assets/image/controller/left.jpg"))})
    let previous = new THREE.Mesh( geometry, material )
    previous.name = "previous"
    previous.position.set(ctrlX - 2, ctrlY + 0.2, ctrlZ - 2)
    previous.rotation.y += 1.5
    scene.add(previous)

    // next
    geometry = new THREE.CylinderGeometry( 1.5, 1.5, 2, 30 )
    material = new THREE.MeshStandardMaterial({ map: loaderTexture.load(require("../assets/image/controller/right.jpg"))})
    let next = new THREE.Mesh( geometry, material )
    next.name = "next"
    next.position.set(ctrlX + 2, ctrlY + 0.2, ctrlZ - 2)
    next.rotation.y += 1.5
    scene.add(next)

    geometry = new THREE.BoxGeometry( 5, 1.8, 2, 30 )
    material = new THREE.MeshPhongMaterial({ color: 0x911010 })
    let red_light = new THREE.Mesh( geometry, material )
    red_light.position.set(48.7, 4.7, 30)
    scene.add(red_light)

    // Light
    const red_point_light = new THREE.PointLight( 0xb71c1c, 0, 100 )
    red_point_light.position.set(49, 4, 32)
    scene.add( red_point_light )

    const point_light = new THREE.PointLight( 0xffffff, 3, 100 )
    point_light.position.set(-1, 40, 60)
    scene.add( point_light )

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
      
      if (intersects.length > 0 ){
        let item_name = intersects[0].object.name

        // Controller connection
        if (item_name == "previous"){
          buttonEvent()
          clicked(previous)

          local.loadedProject -= 1
          if (local.loadedProject < 0){
            local.loadedProject = local.projects.length - 1
          }      
          local.displayProjet() 
          local.setEvents() 
        }
        else if (item_name == "next"){
          buttonEvent()
          clicked(next)
          local.loadedProject += 1
          if (local.loadedProject > local.projects.length - 1){
            local.loadedProject = 0
          }      
          local.displayProjet()  
          local.setEvents() 
        }
        else if (item_name == "visit"){
          buttonEvent()
          clicked(this.visit)
          if (local.projects[local.loadedProject].page_link){
            window.open(local.projects[local.loadedProject].page_link)
          }
        }
        else if (item_name == "git"){
          buttonEvent()
          clicked(git)
          if (local.projects[local.loadedProject].git_link){
            window.open(local.projects[local.loadedProject].git_link)
          }
        }
        else if (item_name == "power"){
          buttonEvent()
          clicked(power)
          local.$emit('exit')
        }
      } 

      // Red Light flashing
      function buttonEvent (){        
        // FlashLight
        red_point_light.intensity = 10
          setTimeout(() => {        
            red_point_light.intensity = 0
          }, 100)

        // If touchScreen, Block mousemove
        local.checkTouchScreen()
      }

      function clicked(item){        
        for (let i = 0; i < 10; i++){
          // Down
          setTimeout(() => {
            item.position.y -= 0.015
          }, i*10)
        }
        setTimeout(() => {
          // Up
          for (let i = 0; i < 10; i++){
            setTimeout(() => {
              item.position.y += 0.015
            }, i*10)
          }
        }, 100)
      }
    }

    // Animation
    this.animate = function () {
      if (local.run){
        requestAnimationFrame(local.animate)

        if(camera){
          local.moveCamera(camera)
        }
        renderer.render(scene, camera)
      }
    }

    // Check for TouchScreen
    this.usingTouchScreen = 
    ( 'ontouchstart' in window ) || 
    ( navigator.maxTouchPoints > 0 ) ||
    ( navigator.msMaxTouchPoints > 0 )

    // --- Camera animation  ---
    let projects = document.getElementById('projects')
    projects.addEventListener('mousemove', event => {
      let timeout = 10
      if (this.usingTouchScreen){
        timeout = 25
      }
      setTimeout(() => {
        if (!this.blockMouseControl){
          let ratioX = window.innerWidth / this.angleX
          let ratioY = window.innerHeight / this.angleY

          this.cameraRotationX = this.screenOffset + (((window.innerWidth - event.offsetX) / ratioX) - this.angleX / 2)
          this.cameraRotationY = ((window.innerHeight - event.offsetY) / ratioY) - this.angleY / 2
        }
      }, timeout)      
    })  
    
    // BlockMouseHover
    let arrow_holder = document.getElementById('arrow_holder')
    arrow_holder.addEventListener('mouseenter', () => {
      this.blockMouseControl = true
    }) 
    arrow_holder.addEventListener('mouseleave', () => {
      this.blockMouseControl = false
    })

    this.setEvents()
  }
}
</script>

<style scoped>
#projects
{
  position: absolute;
  height: 100vh;
  width: 100vw;
}
#holder
{
  display: grid;
}
#scene_holder
{
  position: relative;
  grid-column: 1;
  grid-row: 1;
}
#description_holder
{
  position: absolute;
  grid-column: 1;
  grid-row: 1;
  width: 100%;
  height: 100%;

  display: flex;
  justify-content: flex-end;
  pointer-events: none;
  opacity: 0;
  transition: opacity 500ms;
}
#project_description
{
  width: 30em;
  height: min-content;
  background-color: rgb(20, 20, 20, 0.8);
  margin: 2vw;
  z-index: 2;

  font-size: 1.5em;
  padding: 1em;

  -webkit-touch-callout: none;
  -webkit-user-select: none; 
  -khtml-user-select: none;
  -moz-user-select: none;
  -ms-user-select: none;
  user-select: none; 
}
#description_title
{
  font-size: 2.5em;  
  margin: 0px;
}
#description_pannel
{
  font-size: 1.5em;  
  transition-duration: 500ms;
}
#icon_stack_holder
{
  display: flex;
  flex-direction: row;
  justify-content: flex-start;
}
.icon_holder
{
  margin: 0.8em;
  height: 1.5em;
  width: 1.5em;
  pointer-events: all;
}
.icon_holder img
{
  height: 100%;
  width: 100%;
  object-fit: contain;
}
.techno_holder
{
  display: flex;
  justify-content: center;
  align-items: center;

  flex-direction: column;
}
.techno_holder p
{
  position: absolute;
  text-align: center;

  opacity: 0;
  transition-duration: 500ms;
}
#arrow_holder
{
  height: 2em;
  width: 2em;
  margin-left: auto;
  pointer-events: all;
}
#arrow_holder img 
{
  object-fit: contain;
  width: 100%;
  height: 100%;
  opacity: 0.4;
}
#message
{
  position: absolute;
  height: 50vh;
  width: 5px;
  margin: 20vw;
  margin-top: 10vw;
  background-color: white;
  z-index: 3;
  pointer-events: none;
  opacity: 0;

  transition-duration: 500ms;
}
#message p
{
  font-size: 2em;
  margin: 1em;
  margin-top: 7em;
}
#arrow
{  
  margin: 1em;
  height: 3em;
  width: 3em;
}
#arrow img
{
  height: 100%;
  width: 100%;
  object-fit: contain;
}
@media screen and (max-width: 1000px) {
  #project_description
  {
    width: 100%;
    margin: 0px;
  }
  #project_description p
  {
    margin: 10px;
  }
  .icon_holder
  {
    margin: 10px;
    height: 1em;
    width: 1em;
  }    
  .techno_holder p
  {
    font-size: 0.7em;
  }
}
</style>

<style module>
@keyframes slide {
  0%{
    transform: translateY(-20vh);
    opacity: 0;
  }
  50%{
    transform: translateY(0);
    opacity: 1;

  }
  100%{
    transform: translateY(20vh);
    opacity: 0;
  }  
}
</style>