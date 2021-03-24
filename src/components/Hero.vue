<template>
  <div id="hero">
    <div id="hero_header">
      <a href="" id="projects_button">
        Projets
      </a>
    </div>
    <div id="hero_body">
      <h1>
        Simon
      </h1> 
      <p>
        creative front end
      </p>
    </div>
  </div>
</template>

<script>
export default {
  name: 'Hero',
}
</script>

<style scoped>
#hero_header
{
  display: flex;
  justify-content: flex-end;
}
#hero_body
{
  display: flex;
  justify-content: center;
  align-items: center;
  flex-direction: column;

  width: 100vw;
  height: 100vh;
}
#projects_button
{
  position: absolute;
  margin: 3em;

}
</style>
