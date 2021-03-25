<template>
  <div data-scroll-container>    
    <Hero data-scroll-section />
    <Love data-scroll-section />
    <Imagine data-scroll-section id="imagine" ref="refimagine" />
    <Cloud data-scroll-section />
    <Build data-scroll-section id="build" ref="refbuild"/>
    <Funny data-scroll-section id="funny" ref="reffunny" />
    <div id="testboy">
    </div>
  </div>
</template>

<script>
import Hero from '../components/Hero.vue'
import Love from '../components/Love.vue'
import Imagine from '../components/Imagine.vue'
import Cloud from '../components/Cloud.vue'
import Build from '../components/Build.vue'
import Funny from '../components/Funny.vue'

import LocomotiveScroll from 'locomotive-scroll';

export default {
  name: 'Main',
  components: { Hero, Love, Imagine, Build, Cloud, Funny },
  daat(){
    return {
      scroll: null
    }
  },
  mounted() {    
    this.scroll = new LocomotiveScroll({
      el: document.querySelector('[data-scroll-container]'),
      smooth: true,
      lerp: 0.07,
      smartphone: {
        smooth: true,
        direction: 'vertical',
        gestureDirection: 'vertical',
        lerp: 0,
      },
      tablet: {
        smooth: true, 
        direction: 'vertical',
        gestureDirection: 'vertical',
      },
    })
    
    let matter_activated = false
    let letter_activated = false
    this.scroll.on('scroll', event => {
      let half_win = window.innerHeight / 2

      let updating = false
      if (!updating){
        if( half_win > document.getElementById('funny').getBoundingClientRect().top ){
          if (!letter_activated){
            letter_activated = true
            this.$refs.reffunny.loadLetters()
          }
        }        
        else if( half_win > document.getElementById('build').getBoundingClientRect().top ){
          if (!matter_activated){
            matter_activated = true
            this.$refs.refbuild.loadScene()
          }
        }    
        else if( window.innerHeight > document.getElementById('imagine').getBoundingClientRect().top ){
          this.$refs.refimagine.updateScene(event)
        }

        updating = true
        setTimeout(() => {
          updating = false
        }, 200)
      }   
    })
  }
}
</script>

<style scoped>
@import '../locomotive/locomotive-scroll.css';
#testboy
{
  height: 200vh;
  width: 100vw;
  background-color: rgb(218, 151, 64);
}
</style>
