<template>
  <div id="hero">
    <div id="center_bubble">
      <span id="bubble1">      
      </span>
      <span id="bubble2">      
        <span id="lil_holder">
          <span id="lil_bubble">          
          </span>
        </span>
      </span>
      <span id="bubble3">      
      </span>
    </div>
    <div id="hero_header">
      <div id="button_background">
      </div>
      <p id="projects_button">
        Projects
      </p>
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
      <div id="sub_title" data-scroll data-scroll-speed="-1">
        <div id="creative_stack">
        </div>
        <p>
          front-end
        </p>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'Hero', 
  methods: {
    open_project() {
      this.$emit('open_project')
    }
  },
  mounted(){
    // Create text animation
    let color_stack =  ["e53935", "8e24aa", "3949ab", "039be5", "00897b", "7cb342", "fdd835", "fb8c00"]
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

    document.getElementById('lil_holder').style.setProperty('--rotate', this.$style["rotate"])

    // Create bubbles
    let hero = document.getElementById('hero')
    let bubble1 = document.getElementById('bubble1')
    let bubble2 = document.getElementById('bubble2')
    let bubble3 = document.getElementById('bubble3')

    let title_white = document.getElementById('title_white')
    let plane_gray = document.getElementById('plane_gray')
    let title_translate = document.getElementById('title_translate')

    // Button Hover
    let projects_button = document.getElementById('projects_button')
    let button_background = document.getElementById('button_background')

    let button_rect = projects_button.getBoundingClientRect()

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
      if (
        button_rect.left < event.offsetX && 
        event.offsetX < button_rect.right &&
        button_rect.top < event.offsetY &&
        event.offsetY < button_rect.bottom
      ){
        projects_button.style.color = "#262626"
        button_background.style.clipPath = "circle(100% at 50% 50%)"
      }
      else{
        projects_button.style.color = "#ffffff"
        button_background.style.clipPath = "circle(0% at 50% 50%)"
      }
    })

    // Open project 3D Scene
    hero.addEventListener('click', event => {
      let projects_button = document.getElementById('projects_button')

      let button_rect = projects_button.getBoundingClientRect()

      if (
        button_rect.left < event.offsetX && 
        event.offsetX < button_rect.right &&
        button_rect.top < event.offsetY &&
        event.offsetY < button_rect.bottom
      ){
        this.open_project()
      }
    })

    // Set Default pos
    let x = (window.innerWidth * 0.35)
    let y =  - (window.innerHeight * 0.25)
    bubble1.style.transform = `translate(${-x}px, ${-y}px)`
    bubble2.style.transform = `translate(${-y * 0.8}px, ${-x * 0.5}px)`
    bubble3.style.transform = `translate(${-x / 2}px, ${-y / 2}px)`
  }
}
</script>

<style scoped>
#hero_header
{
  display: grid;
  justify-content: flex-end;
  pointer-events: none;
  width: 100vw;
  pointer-events: none !important;
}
#hero_body
{
  display: flex;
  justify-content: center;
  align-items: center;
  flex-direction: column;

  width: 100vw;
  pointer-events: none !important;
}
#button_background
{
  background-color: white;
  pointer-events: none;

  grid-column: 1;
  grid-row: 1;
  margin: 2em;

  clip-path: circle(0% at 50% 50%);
  transition-duration: 500ms;
  border-radius: 20px;
}
#projects_button
{
  position: relative;
  margin: 2em;
  font-size: 2em;

  grid-column: 1;
  grid-row: 1;

  transition-duration: 500ms;
}
/* Titles */
#hero_title_holder
{
  display: grid;
  margin-right: 20vw;
}
#hero_title_holder p
{  
  font-size: 20vw;
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
  font-family: saonara;
  margin-left: 20vw;
}
#sub_title p
{
  font-size: 6vw;
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
  width: 100%;
  height: 100%;
  pointer-events: none;

  z-index: 2;
}
#bubble1
{
  position: absolute;
  height: 2em;
  width: 2em;
  background-color: #ffbe0b;
  border-radius: 50%;
  transition-duration: 600ms;
  transition-timing-function: ease-out;
}
#bubble2
{
  position: absolute;
  height: 3em;
  width: 3em;
  background-color: #ff006e;
  border-radius: 50%;
  transition-duration: 900ms;
  transition-timing-function: ease-out;
}
#bubble3
{
  position: absolute;
  height: 1em;
  width: 1em;
  background-color: #8338ec;
  border-radius: 50%;

  transition-duration: 300ms;
  transition-timing-function: ease-out;
}
#lil_bubble
{
  position: absolute;
  height: 1.5em;
  width: 1.5em;
  background-color: #3a86ff;
  border-radius: 50%;
  top: 2em;
  left: 2em;
}
#lil_holder
{
  position: absolute;
  animation: var(--rotate) 5s infinite linear;
}
@media screen and (max-width: 1000px){
  #projects_button
  {
    margin: 1em;
    font-size: 1.5em;
  }
  #hero_body
  {
    margin: 15vh 0vh;
  }
  #sub_title p
  {    
    font-size: 10vw;
  }
}
</style>

<style>
#creative_stack p
{
  grid-column: 1;
  grid-row: 1;
  
  font-size: 6vw;
  padding: 0.5em;
  margin: 0px;
}
@media screen and (max-width: 1000px){
  #creative_stack p
  {
    font-size: 10vw;
  }
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
@keyframes rotate {  
  0%{    
    transform: rotate(0deg) scale(1);
  }
  50%{    
    transform: rotate(180deg) scale(0);
  }
  100%{    
    transform: rotate(360deg) scale(1);
  }
}
</style>