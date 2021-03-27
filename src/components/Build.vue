<template>
  <div id="build">
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

      let stack = [
        {
          url: require("../assets/image/buildTitle/b.png"),
          width: 390,
          height: 560
        },
        {
          url: require("../assets/image/buildTitle/u.png"),
          width: 315,
          height: 428
        },
        {
          url: require("../assets/image/buildTitle/i.png"),
          width: 19,
          height: 592
        },
        {
          url: require("../assets/image/buildTitle/l.png"),
          width: 46,
          height: 577
        },
        {
          url: require("../assets/image/buildTitle/d.png"),
          width: 394,
          height: 600
        }]

      for (let i = 0; i < stack.length; i++){
        this.createLetter(stack[i], i)
      }
    },
    createItem(w, h){
      var item = this.Bodies.rectangle(w, h, 100, 100)
      this.World.add(this.engine.world, [item])
    },
    createLetter(letter, pos){
      let widthPart = window.innerWidth / 7
      let final_height = window.innerHeight / 2

      let ratio = (letter.height / final_height)
      let final_width = letter.width / ratio
      
      let item = this.Bodies.rectangle(widthPart * (pos + 1), 500, final_width, final_height)
      item.render.sprite.texture = letter.url
      item.render.sprite.xScale = 1 / ratio
      item.render.sprite.yScale = 1 / ratio

      console.log(ratio)
      
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
#matter_scene
{
  grid-column: 1;
  grid-row: 1;
}
</style>