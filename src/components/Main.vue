<script>
import axios from 'axios';
import SearchBar from './partials/SearchBar.vue';

export default {
  name: "Main",

  components: {
    SearchBar,
  },

  data() {
    return {
      status: 'new',
      interval: null,

      pokemon: {},
      pokemonArr: [],
    };
  },

  methods: {
    search(input) {
      if (input === '') return;
      this.status = 'loading';
      clearInterval(this.interval);

      axios.get('https://pokeapi.co/api/v2/pokemon/' + input)
        .then( res => {
          this.pokemon = {};
          this.pokemon.name = res.data.name;
          this.pokemon.height = res.data.height;
          this.pokemon.weight = res.data.weight;
          this.pokemon.types = res.data.types;
          this.pokemon.stats = res.data.stats;
          this.pokemon.sprites = res.data.sprites;
          
          this.status = 'result';
          this.interval = setInterval(this.animation, 1000);
        })
        .catch( () => {
          this.pokemon = {};
          
          this.status = 'error';
        })

    },
    
    add(pokemon) {
      this.pokemonArr.push(pokemon.name);

      localStorage.setItem('pokemonArr', JSON.stringify(this.pokemonArr))
    },
    
    remove(pokemon) {
      const pokemonIndex = this.pokemonArr.indexOf(pokemon.name);
      this.pokemonArr.splice(pokemonIndex, 1);

      localStorage.setItem('pokemonArr', JSON.stringify(this.pokemonArr))
    },



    baseStats(stat) {
      let statEdit = stat * 100 / 255;
      return statEdit.toString() + '%';
    },

    animation() {
      const images = document.querySelectorAll('.left .image img');

      images.forEach( image => {
        image.classList.toggle('active');
      })
    }

  },

  mounted() {
    if (JSON.parse(localStorage.getItem('pokemonArr')) !== null) {
      this.pokemonArr = JSON.parse(localStorage.getItem('pokemonArr'));
    }
  },
};
</script>


<template>
  <main>
    <div class="container">

      <div class="left">

        <SearchBar @search="search" @add="add" @remove="remove" :pokemon="pokemon" :pokemonArr="pokemonArr" :status="status"/>

        <div class="image">
          <div class="content">
            <img v-if="status === 'new' || status === 'error'" class="favicon" src="../assets/favicon.png" alt="">
            <p v-else-if="status === 'loading'">Loading...</p>
            <div v-else-if="status === 'result'">
              <img class="pokemon active" :src="pokemon.sprites.front_default" alt="front_default">
              <img class="pokemon" :src="pokemon.sprites.back_default" alt="back_default">
            </div>
          </div>
        </div>

        <div class="info">

          <p v-if="status === 'new'">Let's start the Pokemon Hunt!</p>

          <p v-else-if="status === 'loading'">Loading...</p>

          <p v-else-if="status === 'error'">No valid pokemon selected!</p>

          <div v-else-if="status === 'result'"> 
            <p class="content">
              <strong>Name: </strong>
              <span>{{ pokemon.name }}</span>
            </p>
            <p class="content">
              <strong>Height: </strong>
              <span>{{ pokemon.height }}</span>
            </p>
            <p class="content">
              <strong>Weight: </strong>
              <span>{{ pokemon.weight }}</span>
            </p>
            <p class="content">
              <strong>Type: </strong>
              <ul>
                <li v-for="(item, index) in pokemon.types" :key="index">
                  {{ item.type.name }}
                </li>
              </ul>
            </p>
            <div class="stats">
              <p>
                <strong>Stats:</strong>
              </p>
              <ul>
                <li v-for="item in pokemon.stats">
                  <span>{{ item.stat.name }}: </span>
                  <div class="chart">
                    <div class="stat-chart" :style="`width: ${baseStats(item.base_stat)}`"></div>
                  </div>
                </li>
              </ul>
            </div>
          </div>

        </div>

      </div>

      <div class="right">
        <div class="my-pokemon">
          <h3>My Pokemons List</h3>
          <ul>
            <li v-for="(pokemon, index) in pokemonArr" :key="index">
              <button @click="search(pokemon)">
                {{ pokemon }}
              </button>
            </li>
          </ul>
        </div>
      </div>

    </div>
  </main>
</template>


<style lang="scss" scoped>

.container {
  width: 90%;
  max-width: 1200px;
  margin: auto;
  border: 14px solid #8b051d;
  display: flex;
  border-radius: 7%;
  background-color: #dc0a2d;
  box-shadow: 0px 0px 30px 10px rgba(0, 0, 0, 0.1);

  .left, .right {
    width: 50%;
  }

  .left {
    padding: 20px 40px;
    border-right: 7px solid #8b051d;

    
    .image {
      height: 200px;
      width: 300px;
      margin: 0 auto;
      margin-bottom: 20px;
      border: 4px solid #8b051d;
      border-radius: 30px;
      overflow: hidden;
      position: relative;
      
      .content {
        display: flex;
        justify-content: center;
        align-items: center;
        border: 10px solid #dedede;
        border-radius: 25px;
        background-color: white;
        width: 100%;
        height: 100%;

        .favicon {
          height: 30px;
        }
        
        .pokemon {
          position: absolute;
          top: 0;
          left: 50%;
          transform: translateX(-50%);
          height: 180px;
          display: none;
          
          &.active {
            display: block;
          }
        }
      }
        
    }
    
    .info {
      min-height: 350px;
      background-color: #51ad60;
      padding: 20px;
      border: 4px solid #8b051d;
      border-radius: 10px;
      text-transform: capitalize;

      .content {
        margin-bottom: 10px;
        display: flex;
        gap: 5px;
        
        ul {
          display: flex;
          gap: 5px;
        }
      }

      .stats {
        li {
          display: flex;
          justify-content: space-between;
          align-items: center;
          margin: 5px 0;

          .chart {
            width: 250px;
            height: 8px;
            border: 1px solid #242424;
            border-radius: 8px;

            .stat-chart {
              background-color: #242424;
              border-radius: 8px;
              height: 100%;
            }
          }
        }
      }
    }

  }

  .right {
    border-left: 7px solid #8b051d;
    padding: 70px 50px;
    
    .my-pokemon {
      background-color: #242424;
      border-radius: 10px;
      height: 550px;
      color: #eee;
      padding: 20px 15px;
      overflow: auto;
      border: 4px solid #8b051d;

      h3 {
        margin-bottom: 20px;
        text-align: center;
      }

      ul {
        padding-left: 20px;
        list-style: square;
        li button {
          margin-bottom: 6px;
          border: none;
          text-transform: capitalize;
          background-color: transparent;
          color: #ccc;
          font-size: 1.1rem;
          cursor: pointer;
          
          &:hover {
            text-decoration: underline;
            color: #fff;
          }
        }
      }
    }
  }

}
</style>