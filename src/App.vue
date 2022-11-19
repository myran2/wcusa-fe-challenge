<template>
  <router-view/>
</template>

<style lang="scss">
body {
  background-color: rgb(227, 240, 244);
  background-attachment: fixed;
}
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;

  max-width: $max-app-width;
  margin: 0 auto;

  .home {
    h1 {
      text-align: center;
      margin: 0;
      margin-top: 3%;
    }
  }
}

.btn {
  width: fit-content;
  text-decoration: none;
  border-radius: 25px;
  display: inline-block;
  border: 3px solid;
  padding: 8px 40px;
}

</style>
