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
  data() {
    return {
      World: Matter.World,
      engine: null,
      Bodies: Matter.Bodies
    }
  },
  methods: {
    loadScene() {
      // create two boxes and a ground
      let h = 0
      let pos_list = [[0, h], [0, h], [100, h], [200, h], [300, h], [400, h], [500, h], [600, h], [700, h], [800, h], 
      [0, h - 100], [100, h - 100], [200, h - 100], [300, h - 100], [400, h - 100], [500, h - 100], [0, h - 200], [100, h - 200], [200, h - 200], [300, h - 200]]
      
      for (let i of pos_list){
        this.createItem(i[0], i[1])
      }    
      
      // Title animation
      let build_title = document.getElementById('build_title')
      build_title.style.opacity = "1"
      build_title.style.transform = "translateY(10vh)"

      build_title.dataset.scroll = ""
      build_title.dataset.scrollSpeed = "-10"
    },
    createItem(w, h){
      var item = this.Bodies.rectangle(w, h, 100, 100)
      this.World.add(this.engine.world, [item])
    }
  },
  mounted(){
    // module aliases
    var Engine = Matter.Engine,
    Render = Matter.Render;
    this.engine = Engine.create()

    let container = document.getElementById("matter_scene")  
    
    // create a renderer
    let render_2d = Render.create({
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

    // mouse
    let mouse = Matter.Mouse.create(render_2d.canvas)
    let mouse_Constraint = Matter.MouseConstraint.create(this.engine, {
      mouse: mouse,
      constraint: {
        render_2d: { visible: false }
      }
    })
    render_2d.mouse = mouse

    // Enable scroll page over the scene
    mouse.element.removeEventListener("mousewheel", mouse_Constraint.mouse.mousewheel)
    mouse.element.removeEventListener("DOMMouseScroll", mouse_Constraint.mouse.mousewheel)

    // Add bodies to the world
    this.World.add(this.engine.world, [ground, mouse_Constraint])

    // run the engine
    Engine.run(this.engine)
    Render.run(render_2d)
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
  transform: translateY(-30vh);
  transition-duration: 500ms;
  margin: 0px;
  opacity: 0;
  
  grid-column: 1;
  grid-row: 1;
  pointer-events: none;
}
#matter_scene
{
  grid-column: 1;
  grid-row: 1;
}
</style>