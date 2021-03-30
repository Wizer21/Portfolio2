<template>
  <div id="app_container">
    <Main @open_project="open_project" id="main"/>
    <Projects @exit="close_project" id="projects" ref="projectsref"/>
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
      let projects = document.getElementById('projects')
      let main = document.getElementById('main')

      projects.style.display = "block"
      this.$refs.projectsref.lookTv()
      main.style.transform = "translate(-100vw, 100vh) skew(25deg, 25deg)"
      main.style.opacity = "0"
    },
    close_project(){
      let main = document.getElementById('main')

      main.style.display = "block"
      main.style.transform = "translate(0vw, 0vh) skew(0deg, 0deg)"
      this.$refs.projectsref.lookAway()
      main.style.opacity = "1"
    }
  }
}
</script>

<style>
@import url('https://fonts.googleapis.com/css2?family=Lexend:wght@100&display=swap');
@font-face {
    font-family: saonara;
    src: url("../src/assets/font/saonara.otf");
}
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
#app_container
{
  display: grid;
}
#projects
{
  display: none;
  grid-column: 1;
  grid-row: 1;
  transition-duration: 3500ms;
}
#main
{
  grid-column: 1;
  grid-row: 1;
  transition-duration: 3500ms;
  z-index: 3;

  background-color: #262626;
}
</style>
