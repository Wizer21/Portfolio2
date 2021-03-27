<template>
  <div id="funny">
    <div id="title_holder">
    </div>
  </div>
</template>

<script>
export default {
  name: 'Funny',
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
    let elem 
    let div = document.createElement('div')
    div.className = "word"    

    let lil_div
    let delay = 0

    let direction = 0
    
    for (let letter of 'Funny Projects'){
      if (letter == " "){ 
        // Second Word
        title_holder.appendChild(div)  

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
          elem.style.transform = "translate(25vw, 0vw)"          
        }
        else if (direction == 1){          
          elem.style.transform = "translate(0vw, 25vw)"   
        }
        else if (direction == 2){          
          elem.style.transform = "translate(-25vw, 0vw)"   
        }
        else if (direction == 3){          
          elem.style.transform = "translate(0vw, -25vw)"   
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
  }
}
</script>

<style scoped>
#funny
{
  height: 100vh;
  width: 100vw;
  display: flex;
  justify-content: center;
  align-items: center;
}
#title_holder
{
  display: flex;
  flex-direction: column;
}
</style>

<style >
.word
{
  display: flex;
  flex-direction: row;
}
.lil_div
{
  overflow: hidden;
}
.letter
{
  font-size: 15vw;
  margin: 0px;
  transition-duration: 1000ms;
  transition-timing-function: ease-in-out;
}
</style>