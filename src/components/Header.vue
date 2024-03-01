<script>

export default {
  name: "Header",
};
</script>


<template>

  <header>
    <img class="favicon" src="../assets/favicon.png" alt="pokedex">
    <h2>
      PokeDex
      <img class="logo" src="../assets/logo-pokemon.png" alt="logo-pokemon">
    </h2>
  </header>

</template>


<style lang="scss" scoped>

header {
  display: flex;
  justify-content: center;
  align-items: center;
  background-color: #333;
  padding: 20px 50px;
  color: #eee;
  height: 80px;
  
  .favicon {
    height: 36px;
  }

  h2 {
    position: relative;
    margin-left: 20px;
    font-size: 2.2rem;
    text-transform: uppercase;
    font-weight: 900;

    .logo {
      height: 30px;
      position: absolute;
      top: -10px;
      right: -65px;
    }
  }
}
</style>