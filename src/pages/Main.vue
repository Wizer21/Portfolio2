<template>
  <div data-scroll-container>    
    <Hero data-scroll-section @open_project="open_project"/>
    <Love data-scroll-section id="love" />
    <Imagine data-scroll-section id="imagine" ref="refimagine" />
    <Cloud data-scroll-section />
    <Build data-scroll-section id="build" ref="refbuild"/>
    <Funny data-scroll-section id="funny" ref="reffunny" />
    <Footer data-scroll-section />
  </div>
</template>

<script>
import Hero from '../components/Hero.vue'
import Love from '../components/Love.vue'
import Imagine from '../components/Imagine.vue'
import Cloud from '../components/Cloud.vue'
import Build from '../components/Build.vue'
import Funny from '../components/Funny.vue'
import Footer from '../components/Footer.vue'

import LocomotiveScroll from 'locomotive-scroll';

export default {
  name: 'Main',
  components: { Hero, Love, Imagine, Build, Cloud, Funny, Footer },
  methods: {
    open_project(){
      this.$emit('open_project')
    }
  },
  data(){
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
          this.$refs.refbuild.stop()
        }        
        else if( half_win > document.getElementById('build').getBoundingClientRect().top ){
          if (!matter_activated){
            matter_activated = true
            this.$refs.refbuild.loadScene()
          }
          this.$refs.refbuild.start()
          this.$refs.refimagine.stop()
        }    
        else if( window.innerHeight > document.getElementById('imagine').getBoundingClientRect().top ){
          this.$refs.refimagine.updateScene(event)
          this.$refs.refbuild.stop()
          this.$refs.refimagine.start()
        }
        else if( window.innerHeight > document.getElementById('love').getBoundingClientRect().top ){
          this.$refs.refimagine.stop()
        }

        updating = true
        setTimeout(() => {
          updating = false
        }, 200)
      }   
    })

    // WIP
    setTimeout(() => {
      this.scroll.update()
    }, 1500)
  }
}
</script>

<style scoped>
@import '../locomotive/locomotive-scroll.css';
</style>
