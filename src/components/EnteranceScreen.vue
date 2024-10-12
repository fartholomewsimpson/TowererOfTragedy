<template>
  <div>
    <div>
      <img alt="Door" src="../assets/entrance.png" />
    </div>
    <div class="buttonContainer">
      <button @click="$emit('enter')">ENTER</button>
    </div>
  </div>
</template>

<script>
export default {
  name: 'EntranceScreen',
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
img {
  margin-top: 50%;
}
h3 {
  margin: 40px 0 0;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
button {
  background-color: rgb(186, 192, 71);
  padding-left: 10px;
  padding-right: 10px;
}
.buttonContainer {
  position: absolute;
  bottom: 15px;
  left: 0;
  right: 0;
}
</style>
