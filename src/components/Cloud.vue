<template>
  <div id="cloud"> 
    <div id="cloud_container" data-scroll data-scroll-speed="2">
      <img :src="require('../assets/image/cloud.png')" alt="cloud">
      <p data-scroll data-scroll-speed="-2" data-scroll-offset="-500, 0">
        and
      </p>
    </div>
  </div>
</template>

<script>
export default {
  name: 'Cloud',
}
</script>

<style scoped>
#cloud
{
  position: absolute;
  z-index: 2;
  pointer-events: none !important;
}
#cloud_container
{
  position: relative;
  width: 100vw;
  height: 150vh;

  top: -85vh;
}
#cloud_container img
{
  object-fit: fill;
  width: 100%;
  height: 100%;
}
#cloud_container p
{
  position: relative;
  font-size: 10vw; 
  margin: 2em;
  top: -125vh;
}
@media screen and (max-width: 1000px){
  #cloud_container p
  {
    font-size: 20vw; 
    top: -105vh;
    margin: 1em;
  }
}
</style>
