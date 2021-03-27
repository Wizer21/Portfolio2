<template>
  <div id="cloud"> 
    <div id="cloud_container" data-scroll data-scroll-speed="4">
      <img :src="require('../assets/image/cloud.png')" alt="cloud">
      <p data-scroll data-scroll-speed="-2">
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
  font-size: 6vw; 
  margin: 2em;
  top: -95vh;
}
</style>
