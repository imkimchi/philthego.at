<template>
  <div class="hello">
    <img id="phone" src="https://i.imgur.com/faBePdc.jpg" alt="">
  </div>
</template>

<script>
export default {
  name: 'HelloWorld',
  props: {
    msg: String
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
  #phone {
    height: 100%;
  }
</style>
