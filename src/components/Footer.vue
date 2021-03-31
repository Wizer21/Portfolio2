<template>
  <div id="footer">
    <p id="footer_text_p1" class="footer_text">
      You want me to join your team ?
    </p>
    <p id="footer_text_p2" class="footer_text">
      Contact me !
    </p>
    <div class="holder" id="gmail_holder">
      <a href="mailto:simon.tessier8@gmail.com?subject=Front-end&body=Hello!">
        <h2>
          Email
        </h2>
        <h2 id="gmail">
          Email
        </h2>
      </a>
    </div>
    <div class="holder" id="twitter_holder">
      <a href="https://twitter.com/Simon349_">
        <h2>
          Twitter
        </h2>
        <h2 id="twitter">
          Twitter
        </h2>
      </a>
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
  },
  mounted(){
    let holders = document.getElementsByClassName('holder')
    for (let holder of holders){
      holder.addEventListener('mouseenter', event => {
        let square = holder.getBoundingClientRect()
        let x_percent = event.offsetX / (square.width / 100)
        let y_percent = event.offsetY / (square.height / 100)

        let child = holder.children[0].children[1]
        child.style.transitionDuration = "0ms"
        child.style.clipPath = `circle(0% at ${x_percent}% ${y_percent}%)`

        setTimeout(() => {          
          child.style.transitionDuration = "1000ms"
          child.style.clipPath = `circle(150% at ${x_percent}% ${y_percent}%)`
        }, 10)
      })
    }
    for (let holder of holders){
      holder.addEventListener('mouseleave', event => {
        let square = holder.getBoundingClientRect()
        let x_percent = event.offsetX / (square.width / 100)
        let y_percent = event.offsetY / (square.height / 100)

        let child = holder.children[0].children[1]
        child.style.transitionDuration = "500ms"
        child.style.clipPath = `circle(0% at ${x_percent}% ${y_percent}%)`
      })
    }
  }
  
}
</script>

<style scoped>
#footer
{
  margin: 15vh 0;

  width: 100vw;
  display: flex;
  justify-content: center;
  align-items: center;
  flex-direction: column;
}
.footer_text
{
  font-size: 2.5vw;
  margin: 0px;
}
#footer_text_p1
{  
  margin-right: 25vw;
}
#footer_text_p2
{
  font-size: 5vw;
  margin-left: 10vw;
}
.holder a
{
  display: grid;
  align-items: center;
  justify-content: center;
  color: #1a1a1a;
}
#gmail_holder
{
  margin-right: 30vw;
}
#twitter_holder
{
  margin-left: 30vw;
}
.holder a h2
{
  grid-column: 1;
  grid-row: 1;
  font-size: 12vw;
  margin: 0px;
}
#gmail
{
  color: #d93025;
  clip-path: circle(0% at 0% 0%)
}
#twitter
{
  color: #1da1f2;
  clip-path: circle(0% at 0% 0%)
}
@media screen and (max-width: 1000px){
  .footer_text
  {
    font-size: 4.5vw;
  }
  #footer_text_p2
  {
    font-size: 8vw;
  }
  .holder
  {
    margin: 5vw 1vw;
  }
  .holder a h2
  {
    font-size: 18vw;    
  }
}
</style>
