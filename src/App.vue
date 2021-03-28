<template>
  <div>
    <Main @open_project="open_project" id="main"/>
    <Projects @exit="close_project" id="projects"/>
  </div>
</template>

<script>
import Main from './pages/Main.vue'
import Projects from './pages/Projects.vue'

export default {
  name: 'App',
  components: { Main, Projects },
  methods:{
    open_project(){
      console.log('inapp')
      document.getElementById('projects').style.display = "block"
      document.getElementById('main').style.display = "none"
    },
    close_project(){
      document.getElementById('projects').style.display = "none"
      document.getElementById('main').style.display = "block"
    }
  }
}
</script>

<style>
@import url('https://fonts.googleapis.com/css2?family=Lexend:wght@100&display=swap');
::-moz-selection {
  color: rgb(44, 44, 44);
  background: rgb(236, 236, 236);
}

::selection {
  color: rgb(44, 44, 44);
  background: rgb(236, 236, 236);
}
body
{
  margin: 0px;
  color: rgb(219, 219, 211);
  font-family: 'Lexend', sans-serif;

  background-color: #262626;
}
#app
{
  overflow: hidden;
}
a
{
  color: rgb(219, 219, 211);
  text-decoration: none;
}
</style>
