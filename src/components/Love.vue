<template>
  <div id="love">
    <h2>
      What I love
    </h2>
    <p>
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
}
#love p
{
  font-size: 15vw;
  margin: 0px;
}
</style>
