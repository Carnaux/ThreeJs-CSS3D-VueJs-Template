<template>
  <div id="panel">
    test element
    <button>click me</button>
  </div>
</template>

<script>
export default {
  name: "htmlPanel",
  components: {},
  data() {
    return {};
  }
};
</script>

<style scoped>
#panel {
  width: 100px;
  height: 100px;
  /* background-color: blue; */
  /* background: rgba(200,200,200,0.15); */
  background: #fafafe;
  font-family: "Lucida Sans", "Lucida Sans Regular", "Lucida Grande",
    "Lucida Sans Unicode", Geneva, Verdana, sans-serif;
  padding: 12px;
  box-sizing: border-box;
  /* opacity: 0.5; */
}
button {
  background: white;
  border: none;
  padding: 6px 12px;
  border-radius: 12px;
}
button:hover{
    background: #1288ff;
    color:#fff;
}
</style>