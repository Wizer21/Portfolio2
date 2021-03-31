<template>
  <div id="funny">
    <div id="title_holder">
      <div id="funny_holder">
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'Funny',
  data(){
    return {
      color_stack: [{
          color: "e53935",
          pos: 8,
          h1: null
        },
        {
          color: "8e24aa",
          pos: 7,
          h1: null
        },
        {
          color: "3949ab",
          pos: 6,
          h1: null
        },
        {
          color: "039be5",
          pos: 5,
          h1: null
        },
        {
          color: "00897b",
          pos: 4,
          h1: null
        },
        {
          color: "7cb342",
          pos: 3,
          h1: null
        },
        {
          color: "fdd835",
          pos: 2,
          h1: null
        },
        {
          color: "fb8c00",
          pos: 1,
          h1: null
        }
      ]
    }   
  },
  methods: {
    loadLetters() {
      let stack = document.getElementsByClassName('letter')
      for (let letter of stack){
        letter.style.transform = "translate(0vw, 0vw)"
      }
    }
  },
  mounted(){
    let title_holder = document.getElementById('title_holder')
    let funny_holder = document.getElementById('funny_holder')
    let elem 
    let div = document.createElement('div')
    div.className = "word"
    div.id = "funny_word"    

    let lil_div
    let delay = 0

    let direction = 0
    
    for (let letter of 'Funny Projects'){
      if (letter == " "){ 
        // Second Word
        funny_holder.appendChild(div)  

        div = document.createElement('div')
        div.className = "word"
      }
      else{
        // Create Letter 
        elem = document.createElement('h2')
        elem.textContent = letter   

        // transitionDelay
        elem.style.transitionDelay = `${delay}ms`
        delay += 50
        elem.className = "letter"    

        // Direction
        if(direction == 0){
          elem.style.transform = "translate(40vw, 0vw)"          
        }
        else if (direction == 1){          
          elem.style.transform = "translate(0vw, 40vw)"   
        }
        else if (direction == 2){          
          elem.style.transform = "translate(-40vw, 0vw)"   
        }
        else if (direction == 3){          
          elem.style.transform = "translate(0vw, -40vw)"   
          direction = -1
        }
        direction += 1

        // Create Letter Container
        lil_div = document.createElement('div')
        lil_div.className = "lil_div"
        lil_div.appendChild(elem)

        div.appendChild(lil_div)
      }
    }
    title_holder.appendChild(div)  

    // funny colors
    let funny_word = document.getElementById('funny_word')

    let latency = 150
    for (let item of this.color_stack){
      elem = document.createElement('h2')
      elem.className = "funny_color"
      elem.textContent = "Funny"
      elem.style.color = `#${item.color}`
      elem.style.zIndex = item.pos
      elem.style.transform = `translate(${25 - (elem.pos * 2)}px, ${25 - (elem.pos * 2)}px)`
       
      elem.style.transitionDuration = `${latency}ms`
      latency += 150

      item.h1 = elem

      funny_holder.appendChild(elem)
    }

    // Drag animation
    let funny = document.getElementById('funny')

    let holded = false
    let mouse_in = false
    
    funny.addEventListener('mousedown', () => {   
      if(mouse_in){
        holded = true
      }   
    })
    funny.addEventListener('mouseup', () => {    
      holded = false 
      resetPos() 
    })
    funny.addEventListener('mousemove', event => {
      let local_x = event.offsetX - (window.innerWidth / 2)
      let local_y = event.offsetY - (window.innerHeight / 2)
      let rect = funny_word.getBoundingClientRect()

      if (
        rect.left < event.offsetX && 
        event.offsetX < rect.right &&
        rect.top < event.offsetY &&
        event.offsetY < rect.bottom ||
        holded
        ){
        if (!holded){
          funny_word.style.transform = `translate(0px, -15px)`
        }
        mouse_in = true

        if (holded){
          funny_word.style.transform = `translate(${local_x}px, ${local_y}px)`

          let slider = 0.6
          for (let elem of this.color_stack){            
            elem.h1.style.transform = `translate(${local_x * slider}px, ${local_y * slider}px)`

            slider -= 0.25
          }
        }
      }
      else {
        mouse_in = false
        resetPos()
      }
    })

    let local = this
    function resetPos(){
      funny_word.style.transform = "translate(0px, 0px)"

      for (let elem of local.color_stack){          
        elem.h1.style.transform = `translate(${25 - (elem.pos * 2)}px, ${25 - (elem.pos * 2)}px)`
      }
    }
  }
}
</script>

<style scoped>
#funny
{
  width: 100vw;
  display: flex;
  justify-content: center;
  align-items: center;
  margin: 5vh 0;
}
#title_holder
{
  display: flex;
  flex-direction: column;
  pointer-events: none;
}
#funny_holder
{
  display: grid;
}
</style>

<style>
.word
{
  display: flex;
  flex-direction: row;
  font-size: 5vw;
  pointer-events: none !important;
}
.lil_div
{
  overflow: hidden;
  pointer-events: none;
}
.letter
{
  margin: 0px;
  transition-duration: 1000ms;
  transition-timing-function: ease-in-out;

  -webkit-touch-callout: none;
  -webkit-user-select: none; 
  -khtml-user-select: none;
  -moz-user-select: none;
  -ms-user-select: none;
  user-select: none; 
  pointer-events: none;
}
#funny_word{
  font-size: 15vw;

  transition-duration: 400ms;
  transition-timing-function: ease-out;
  z-index: 9;
}
.funny_color{
  position: absolute;
  font-size: 22.7vw;
  margin: 0px;

  grid-column: 1;
  grid-row: 1;

  transition-timing-function: ease-out;
  
  -webkit-touch-callout: none;
  -webkit-user-select: none; 
  -khtml-user-select: none;
  -moz-user-select: none;
  -ms-user-select: none;
  user-select: none; 

  pointer-events: none;
}
@media screen and (max-width: 1000px){
  #funny_word{
    font-size: 20vw;
  }
  .funny_color
  {
    font-size: 17vh;
  }
}
</style>