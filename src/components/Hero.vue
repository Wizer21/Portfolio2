<template>
  <div id="hero">
    <div id="center_bubble">
      <span id="bubble1">      
      </span>
      <span id="bubble2">      
      </span>
      <span id="bubble3">      
      </span>
    </div>
    <div id="hero_header">
      <a href="" id="projects_button">
        Projets
      </a>
    </div>
    <div id="hero_body">
      <div id="hero_title_holder">
        <p id="title_white">
          Simon
        </p> 
        <div id="plane_gray">
        </div> 
        <p id="title_translate">
          Simon
        </p> 
      </div>
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
    let bubble2 = document.getElementById('bubble2')
    let bubble3 = document.getElementById('bubble3')

    let title_white = document.getElementById('title_white')
    let plane_gray = document.getElementById('plane_gray')
    let title_translate = document.getElementById('title_translate')

    let last_x = 0
    let last_y = 0

    hero.addEventListener('mousemove', event => {
      let square = hero.getBoundingClientRect()
      let x = event.offsetX - square.width/2
      let y = event.offsetY - square.height/2

      bubble1.style.transform = `translate(${-x}px, ${-y}px)`
      bubble2.style.transform = `translate(${-y * 0.8}px, ${-x * 0.5}px)`
      bubble3.style.transform = `translate(${-x / 2}px, ${-y / 2}px)`

      // Main Title effect
      let title_rect = title_white.getBoundingClientRect()
      plane_gray.style.height = `${title_rect.height}px`
      plane_gray.style.width = `${title_rect.width}px`

      if (
        title_rect.left < event.offsetX && 
        event.offsetX < title_rect.right &&
        title_rect.top < event.offsetY &&
        event.offsetY < title_rect.bottom
        ){
        // Gray circle
        let local_x = event.offsetX - title_rect.left
        let local_y = event.offsetY - title_rect.top

        let x_percent = local_x / (title_rect.width / 100) 
        let y_percent = local_y / (title_rect.height / 100) 
        
        plane_gray.style.clipPath = `circle(20% at ${x_percent}% ${y_percent}%)`

        // Text translate
        title_translate.style.transform = `translate(${local_x - last_x}px, ${local_y - last_y}px)`
        title_translate.style.clipPath = `circle(20% at ${x_percent}% ${y_percent}%)`
        
        last_x = local_x
        last_y = local_y
      }
      else{
        // Reset positions
        plane_gray.style.clipPath = `circle(0% at 50% 50%)`

        title_translate.style.transform = `translate(0px, 0px)`
        title_translate.style.clipPath = `circle(0% at 50% 50%)`
      }
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
/* Titles */
#hero_title_holder
{
  display: grid;
}
#hero_title_holder p
{  
  font-size: 15em;
  margin: 0px;

  grid-column: 1;
  grid-row: 1;
}
#title_translate
{
  transition-duration: 300ms;
  transition-timing-function: ease-out;
}
#plane_gray
{
  position: absolute;

  background-color: #262626;
  transition-duration: 300ms;
  transition-timing-function: ease-out;
}
/* Subs */
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
  grid-template-rows: repeat(1, 1fr);
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
  pointer-events: none;
}
#bubble1
{
  position: absolute;
  height: 2em;
  width: 2em;
  background-color: red;
  border-radius: 50%;
  transition-duration: 600ms;
  transition-timing-function: ease-out;
  mix-blend-mode: hard-light;
}
#bubble2
{
  position: absolute;
  height: 3em;
  width: 3em;
  background-color: rgb(116, 77, 221);
  border-radius: 50%;
  transition-duration: 900ms;
  transition-timing-function: ease-out;
  mix-blend-mode: hard-light;
}
#bubble3
{
  position: absolute;
  height: 1em;
  width: 1em;
  background-color: rgb(101, 113, 223);
  border-radius: 50%;

  transition-duration: 300ms;
  transition-timing-function: ease-out;
  mix-blend-mode: hard-light;
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