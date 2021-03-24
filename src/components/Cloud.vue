<template>
  <div id="cloud">
    <div id="cloud_container" data-scroll data-scroll-speed="5">
      <img :src="require('../assets/image/cloud.png')" alt="cloud">
      <p>
        And
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
  height: 0vh;
  width: 100vw;
}
#cloud_container
{
  position: absolute;
  width: 100vw;
  height: 150vh;
  pointer-events: none;
  z-index: 3;

  display: grid;
}
#cloud_container img
{
  object-fit: fill;
  height: 100%;
  width: 100%;
  transform: translateY(-150vh);

  grid-column: 1;
  grid-row: 1;
}
#cloud_container p
{
  grid-column: 1;
  grid-row: 1;
  font-size: 6vw;

  margin: 2em;
}
</style>
