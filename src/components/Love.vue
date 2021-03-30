<template>
  <div id="love">
    <div id="love_title">
      <h2>
        What I
      </h2>
      <div id="love_holder" >
        <h2 id="love_text_placeholder">
          love
        </h2>
        <div id="div_slider" data-scroll data-scroll-speed="3" data-scroll-direction="horizontal">
          <h2 id="love_text" data-scroll data-scroll-speed="-3" data-scroll-direction="horizontal">
            love
          </h2>
        </div>
      </div>
    </div>
    <p id="is_text">
      is
    </p>
  </div>
</template>

<script>
export default {
  name: 'Love',
}
</script>

<style scoped>
#love
{
  display: flex;
  justify-content: center;
  align-items: center;
  flex-direction: column;

  width: 100vw;
}
#love h2
{
  font-size: 15vw;
  margin: 2vw;
}
#love_title
{
  display: flex;
  flex-direction: row;
}
#love_holder
{ 
  display: grid;
  font-family: saonara;
}
#love_text_placeholder
{
  color: transparent;
  grid-column: 1;
  grid-row: 1;
}
#div_slider
{
  grid-column: 1;
  grid-row: 1;
  overflow: hidden;

  clip-path: polygon(25% 0%, 100% 0%, 75% 100%, 0% 100%);
}
#love_text
{
  position: absolute;
  color: #f44336;
}
/* Is text */
#is_text
{
  font-size: 15vw;
  margin-top: 2vh;
  margin-bottom: 0px;
  margin-left: 50vw
}
</style>
