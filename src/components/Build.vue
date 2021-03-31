<template>
  <div id="build">
    <h2 id="build_title">
      Build
    </h2>
    
    <div id="matter_scene">
    </div>
  </div>
</template>

<script>
import * as Matter from 'matter-js'
export default {
  name: 'Build',
  data(){
    return {
      World: Matter.World,
      Bodies: Matter.Bodies,
      engine: null,
      Render: Matter.Render,
      render_2d: null,
      running: false
    }
  },
  methods: {
    loadScene() {
      // Title animation
      let build_title = document.getElementById('build_title')
      build_title.style.opacity = "1"
      build_title.style.transform = "translateY(0vh)"
      build_title.dataset.scroll = ""
      build_title.dataset.scrollSpeed = "-10"
      
      // create leaves
      for (let i = 0; i < window.innerWidth/80; i++){
        let leaf = this.Bodies.rectangle(dice(0, window.innerWidth), dice(0, window.innerHeight), 50, 50, {        
          frictionAir: 0.2,
        })
        leaf.render.sprite.texture = require('../assets/image/leaf.png')
        this.World.add(this.engine.world, [leaf])
      }
      function dice(min, max){
        return Math.random() * (max - min) + min
      }
    },
    start(){      
      if (!this.running){
        this.running = true
        this.Render.run(this.render_2d)     
      }
    },
    stop(){
      if (this.running){
        this.running = false
        this.Render.stop(this.render_2d)   
      }
    }
  },
  mounted(){
    // module aliases
    var Engine = Matter.Engine

    this.engine = Engine.create()

    let container = document.getElementById("matter_scene")  
    
    // create a renderer
    this.render_2d = this.Render.create({
    element: container,
    engine: this.engine,
    options: {
        wireframes: false,
        height: window.innerHeight,
        width: window.innerWidth,
        background: 'transparent',
    }
    })
    let ground = this.Bodies.rectangle( -200, window.innerHeight, window.innerWidth * 2 + 800, 50, { isStatic: true })
    ground.render.visible = false
    // mouse
    let mouse = Matter.Mouse.create(this.render_2d.canvas)
    let mouse_Constraint = Matter.MouseConstraint.create(this.engine, {
      mouse: mouse,
      constraint: {
        render_2d: { visible: false }
      }
    })
    
    this.render_2d.mouse = mouse
    // Enable scroll page over the scene
    mouse.element.removeEventListener("mousewheel", mouse_Constraint.mouse.mousewheel)
    mouse.element.removeEventListener("DOMMouseScroll", mouse_Constraint.mouse.mousewheel)
    // Add bodies to the world
    this.World.add(this.engine.world, [ground, mouse_Constraint])
    // run the engine
    Engine.run(this.engine)
    this.Render.run(this.render_2d)
  }
}
</script>

<style scoped>
#build
{
  height: 100vh;
  width: 100vw;
  overflow: hidden;
  display: grid;
}
#build_title
{
  font-size: 35vw;
  text-align: center;
  transform: translateY(-50vh);
  transition-duration: 500ms;
  opacity: 0;
  height: min-content;

  margin: 0px;
  margin-top: auto;
  margin-bottom: 10vh;
  
  grid-column: 1;
  grid-row: 1;
  pointer-events: none;
}
#matter_scene
{
  grid-column: 1;
  grid-row: 1;
}
@media screen and (max-width: 1000px){
  #build_title
  {
    margin-bottom: 30vh;
  }
}
</style>
