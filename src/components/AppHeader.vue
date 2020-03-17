<template>
  <header class="header">
      <div class="header__title">
          <h1>The Rick And Morty</h1>
      </div>
  </header>
</template>

<script>
export default {
  name: "AppHeader"
};
</script>

<style scoped lang="scss">
.header__title {
  background-color: #202329;
  margin: 0px 20%;
  
  h1 {
    text-align: center;
    color: #ff9800;
    text-transform: uppercase;
    padding: 10px 0px;
  }
}

</style>
