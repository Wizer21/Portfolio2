<template>
  <div id="footer">
    <p id="text">
      If you want me to join your team you can contact me at  
    </p>
    <div id="mail" @click="clipboard">
      <p id="top_text">        
        simon.tessier8@gmail.com
      </p>
      <p id="bottom_text">        
        simon.tessier8@gmail.com
      </p>
    </div>
  </div>
</template>

<script>
export default {
  name: 'Footer',
  methods: {
    clipboard(){
      navigator.clipboard.writeText('simon.tessier8@gmail.com');
    }
  }
}
</script>

<style scoped>
#footer
{
  height: 100vh;
  display: flex;
  justify-content: center;
  align-items: center;
}
#text
{
  font-size: 2vw;
}
#mail
{
  display: grid;
}
#top_text
{
  font-size: 4em;
  margin: 3vw;
  color: #e53935;

  grid-column: 1;
  grid-row: 1;

  z-index: 2;

  clip-path: polygon(0% 0%, 75% 0%, 50% 100%, 0% 100%);
  transition-duration: 500ms;
  pointer-events: none;
}
#bottom_text
{
  font-size: 4em;
  margin: 3vw;
  color: rgb(26, 26, 26);
  
  grid-column: 1;
  grid-row: 1;
  pointer-events: none;
}
#mail:hover #top_text
{
  clip-path: polygon(0% 0%, 125% 0%, 100% 100%, 0% 100%);  
}
</style>
