<template>
  <section class="container">
    <header>
      <div class="sub-title">POKEMON CARDS</div>
      <div class="title">Gotta catch ‘em all</div>
    </header>

    <div class="is-flex is-justify-content-space-between">
      <div v-for="(pokemon, index) in pokeList" :key="index" class="card">
        <div>
          <figure>
            <img :src="pokemon.imgUrl" :alt="pokemon.name" />
          </figure>
        </div>

        <div class="content-card">
          <div class="header-content">
            <p class="pokemon-title capitalize">{{ pokemon.name }}</p>
          </div>

          <div class="body-content">
            <div class="item capitalize">
              <strong>Abilities:</strong> {{ pokemon.abilities }}
            </div>

            <div class="item capitalize">
              <strong>Types:</strong> {{ pokemon.types }}
            </div>

            <div class="item"><strong>HP:</strong> {{ pokemon.hp }}</div>

            <div class="item">
              <strong>Attack:</strong> {{ pokemon.attack }}
            </div>

            <div class="item">
              <strong>Defense:</strong> {{ pokemon.defense }}
            </div>
          </div>

          <button class="button" @click="openModal(pokemon)">
            Learn More
          </button>
        </div>
      </div>
    </div>

    <PokemonModalCard :isPokeModal="isPokeModal" :pokeModal="pokeModal" />
  </section>
</template>

<script>
import PokemonModalCard from "~/components/pokemon/pokemon-modal-card";

export default {
  components: {
    PokemonModalCard
  },

  data() {
    return {
      pokeNames: ["charmander", "bulbasaur", "squirtle"],
      pokeList: [],
      pokeModal: {},
      isPokeModal: false
    };
  },

  created() {
    this.getPokemon();
  },

  methods: {
    getPokemon() {
      return this.pokeNames.map(pokemon => {
        fetch(`https://pokeapi.co/api/v2/pokemon/${pokemon}`, {
          method: "GET"
        })
          .then(res => {
            return res.json();
          })
          .then(data => {
            this.pokeList.push({
              name: data.name,
              imgUrl: data.sprites.other["official-artwork"].front_default,
              abilities: this.formatAbilities(
                data.abilities.map(item => item.ability.name)
              ),
              types: data.types.map(item => item.type.name)[0],
              hp: data.stats.filter(item => item.stat.name === "hp")[0]
                .base_stat,
              attack: data.stats.filter(item => item.stat.name === "attack")[0]
                .base_stat,
              defense: data.stats.filter(
                item => item.stat.name === "defense"
              )[0].base_stat,
              specialAttack: data.stats.filter(
                item => item.stat.name === "special-attack"
              )[0].base_stat,
              specialDefense: data.stats.filter(
                item => item.stat.name === "special-defense"
              )[0].base_stat,
              speed: data.stats.filter(item => item.stat.name === "speed")[0]
                .base_stat,
              height: data.height,
              weight: data.weight
            });
          });
      });
    },

    formatAbilities(abilities) {
      return abilities.map(ability => ability.replace("-", " ")).join(", ");
    },

    openModal(pokemon) {
      this.isPokeModal = true;
      this.pokeModal = pokemon;
    }
  }
};
</script>

<style lang="scss" scoped>
.container {
  padding-top: 40px;
  padding-bottom: 40px;
}

header {
  margin-bottom: 40px;

  .sub-title {
    font-style: normal;
    font-weight: 900;
    font-size: 12px;
    line-height: 12px;
    letter-spacing: 1px;
    text-transform: uppercase;
    color: #000000;
    margin-bottom: 20px;
  }

  .title {
    font-style: normal;
    font-weight: 900;
    font-size: 48px;
    line-height: 56px;
    letter-spacing: -1px;
    color: #000000;
  }
}

.card {
  margin-right: 40px;
  width: 100%;

  img {
    background-color: #e0e0e0;
  }
}

.header-content {
  margin: 40px 40px 0;

  .pokemon-title {
    font-size: 24px;
    font-weight: 900;
  }
}

.body-content {
  margin: 12px 40px 20px 40px;

  .item {
    font-size: 14px;
    font-weight: 600;
  }
}

.button {
  border: 2px solid #5700ff;
  box-sizing: border-box;
  border-radius: 22px;
  color: #5700ff;
  margin: 0 40px 40px;

  &:hover {
    border-color: #4600cc;
    color: #4600cc;
  }
}

.capitalize {
  text-transform: capitalize;
}

@media screen and (max-width: 375px) {
  .is-flex {
    flex-wrap: wrap;
  }

  .title {
    font-size: 36px !important;
    padding: 0 20px;
  }

  .sub-title {
    padding: 0 20px;
  }

  .card {
    margin-right: 0;
    width: 100%;

    &:not(:last-child) {
      margin-bottom: 24px;
    }
  }
}
</style>
