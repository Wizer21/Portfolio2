<template>
  <div>    
    <Hero />
    <Love />
    <Imagine />
  </div>
</template>

<script>
import Hero from '../components/Hero.vue'
import Love from '../components/Love.vue'
import Imagine from '../components/Imagine.vue'

export default {
  name: 'Main',
  components: { Hero, Love, Imagine }
}
</script>

<style scoped>
</style>
