<template>
  <b-modal v-model="isPokeModal" :width="640" scroll="keep">
    <div class="card">
      <div>
        <div class="is-flex is-align-items-center">
          <div>
            <figure class="image is-128x128">
              <img :src="pokeModal.imgUrl" :alt="pokeModal.name" />
            </figure>
          </div>
          <div class="media-content pokemon-data" :class="pokeModal.name">
            <p class="pokemon-title capitalize">{{ pokeModal.name }}</p>

            <p>
              <strong>Height:</strong> {{ pokeModal.height }}
              <strong>Weight:</strong> {{ pokeModal.weight }}
            </p>

            <p>
              <strong>Special Attack:</strong> {{ pokeModal.specialAttack }}
              <strong>Special Defense:</strong> {{ pokeModal.specialDefense }}
              <strong>Speed:</strong> {{ pokeModal.speed }}
            </p>
          </div>
        </div>
      </div>
    </div>
  </b-modal>
</template>

<script>
export default {
  name: "PokemonModalCard",

  props: {
    isPokeModal: {
      type: Boolean,
      default: false
    },

    pokeModal: {
      type: Object,
      default: () => {}
    }
  }
};
</script>

<style lang="scss" scoped>
.pokemon-title {
  font-size: 24px;
  font-weight: 900;
}

.media-content {
  margin: 0 25px;

  strong:not(:first-child) {
    margin-left: 16px;
  }
}

.capitalize {
  text-transform: capitalize;
}

.pokemon-data.charmander {
  strong {
    color: #eaac7e;
  }
}

.pokemon-data.bulbasaur {
  strong {
    color: #5d9866;
  }
}

.pokemon-data.squirtle {
  strong {
    color: #65a1b3;
  }
}
</style>
