<script>

export default {
  props: {
    pokemon: Object,
    pokemonArr: Array,
    status: String,
  },

  data() {
    return {
      input: '',
    }
  },

  computed: {
    addRemove() {
      return !this.pokemonArr.includes(this.pokemon.name); 
    }
  }, 
}
</script>


<template>

  <div class="search">
    <div class="bar">
      <input v-model="input" @keyup.enter="$emit('search', input)" type="text" placeholder="Search Pokemon...">
      <i @click="$emit('search', input)" class="fa-solid fa-magnifying-glass"></i>
    </div>

    <div v-if="status === 'result'">
      <button v-if="addRemove" class="" @click="$emit('add', pokemon)">
        <img src="../../assets/gotcha.png" alt="gotcha"> 
      </button>

      <button v-else class="remove" @click="$emit('remove', pokemon)">
        <img src="../../assets/battle.png" alt="battle">
        <span>Remove</span>
      </button> 
    </div>
  </div>

</template>


<style lang="scss" scoped>

.search {
  margin-bottom: 20px;
  display: flex;
  align-items: center;
  justify-content: space-between;

  .bar {
    position: relative;
    margin: 5px 0;
    
    input {
      padding: 8px 12px;
      border-radius: 5px;
      border: 4px solid #8b051d;
      width: 280px;
      font-size: 1rem;
      
      &:focus-visible {
        outline: none;
      }
    }

    i {
      position: absolute;
      right: 0;
      top: 2px;
      bottom: 0;
      width: 40px;
      height: calc(100% - 4px);
      border: 2px solid #8b051d;

      display: flex;
      justify-content: center;
      align-items: center;
      background-color: #ddd;
      color: #333;
      cursor: pointer;
      border-radius: 5px;

      
      &:hover {
        color: #fff;
      }
    }
  }
    
  button {
    border: none;
    background-color: transparent;
    transition: 0.3s;
    cursor: pointer;
    
    img {
      height: 40px;
    }
    
    &:hover {
      scale: 1.1;
    }

    &.remove {
      position: relative;
      font-family: 'Designer Block', sans-serif;
      text-transform: uppercase;
      color: #33363a;
      
      
      span {
        position: absolute;
        top: 5px;
        left: 11px;
        font-size: 16px;
      }
    }
  }
}
</style>