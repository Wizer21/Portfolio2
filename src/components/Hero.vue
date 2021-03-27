<template>
  <div id="hero">
    <div id="center_bubble">
      <span id="bubble1">      
      </span>
    </div>
    <div id="hero_header">
      <a href="" id="projects_button">
        Projets
      </a>
    </div>
    <div id="hero_body">
      <h1 id="title">
        Simon
      </h1> 
      <div id="sub_title">
        <div id="creative_stack">
        </div>
        <p>
          front end
        </p>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'Hero',  
  mounted(){
    // Create text animation
    let color_stack =  ["e53935", "8e24aa", "3949ab", "039be5", "039be5", "7cb342", "fdd835", "fb8c00"]
    let creative_stack = document.getElementById('creative_stack')

    let elem 
    let latency = 0
    for (let color of color_stack){
      setTimeout(() => {
        elem = document.createElement('p')
        elem.textContent = "creative"
        elem.style.color = `#${color}`
        elem.style.animation = `${this.$style.sliide} 5000ms infinite`
        creative_stack.appendChild(elem)
      }, latency)
      latency += 325
    }

    // Create bubbles
    let hero = document.getElementById('hero')
    let bubble1 = document.getElementById('bubble1')
    hero.addEventListener('mousemove', event => {
      let square = hero.getBoundingClientRect()
      let x = event.offsetX - square.width/2
      let y = event.offsetY - square.height/2

      bubble1.style.transform = `translate(${-x}px, ${-y}px)`

      console.log(x, y)

    })
  }
}
</script>

<style scoped>
#hero_header
{
  display: flex;
  justify-content: flex-end;
  pointer-events: none;
}
#hero_body
{
  display: flex;
  justify-content: center;
  align-items: center;
  flex-direction: column;

  width: 100vw;
  height: 100vh;
  pointer-events: none;
}
#projects_button
{
  position: absolute;
  margin: 3em;

}
#title
{  
  font-size: 15em;
  margin: 0px;
}
#sub_title
{  
  display: flex;
  flex-direction: row;
}
#sub_title p
{
  font-size: 4em;
  padding: 0.5em;
  margin: 0px;
}
#creative_stack
{
  display: grid;
}
/*  Bubbles */
#center_bubble
{
  position: absolute;
  display: flex;
  justify-content: center;
  align-items: center;
  width: 100vw;
  height: 100vh;
}
#bubble1
{
  position: absolute;
  height: 2em;
  width: 2em;
  background-color: red;
  pointer-events: none;
}

</style>

<style>
#creative_stack p
{
  grid-column: 1;
  grid-row: 1;
  
  font-size: 4em;
  padding: 0.5em;
  margin: 0px;
}
</style>

<style module>
@keyframes sliide {
  0%{    
    clip-path: polygon(-50% 0, -25% 0, 0% 100%, -25% 100%);
  }
  100%{    
    clip-path: polygon(100% 0, 125% 0, 150% 100%, 125% 100%);
  }
}
</style>