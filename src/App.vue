<template>
  <div class="pokemon-container">

    <div class="pokemon-area">
      <div class="pokemon-name">
        <font color="35B7B7">
          No.<input type="text" v-model="pokemonId" id="pokemonId" name="pokemonId" style="border: none; font-size: 36px; color: #35B7B7; width: 100px;" />
          <img src="./components/image/monsterball.png" @click="search" width="5%" />
          <font color="black  ">{{ speciesData.names ? speciesData.names.find(name => name.language.name === 'ja')?.name : pokemonData.name }}</font>
        </font>
      </div>
      <div class="pokemon-image">
        <img v-if="pokemonData.sprites" :src="pokemonData.sprites.front_default" :alt="pokemonData.name ? pokemonData.name : 'ポケモンの画像'" style="width: 200px; height: 200px;" />
      </div>
      <div class="pokemon-detail">{{ speciesData.flavor_text_entries ? speciesData.flavor_text_entries.find(entry => entry.language.name === 'ja')?.flavor_text ?? speciesData.flavor_text_entries.find(entry => entry.language.name === 'en')?.flavor_text : '' }}</div>
      <div class="pokemon-property">
        <div class="species">分類：{{ speciesData.genera ? speciesData.genera.find(genus => genus.language.name === 'ja')?.genus ?? speciesData.genera.find(genus => genus.language.name === 'en')?.genus : '' }}</div>
        <div class="type">タイプ：
          <span v-for="(type, index) in pokemonData.types" :key="index">{{ type.type.name }} </span>
        </div>
        <div class="size">サイズ：{{ pokemonData.height ? pokemonData.height / 10 + ' m' : '' }}</div>
        <div class="ability">特性：
          <span v-for="(ability, index) in pokemonData.abilities" :key="index">{{ ability.ability.name }} </span>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      pokemonId: "",
      pokemonData: {},
      speciesData: {},
    };
  },
  methods: {
    async search() {
      try {
        // 入力されたIDまたは名前を取得
        const id = this.pokemonId.toLowerCase();
        if (!id) {
          alert("ポケモンIDまたは名前を入力してください。");
          return;
        }

        // ポケモンの基本情報を取得
        const response = await fetch(`https://pokeapi.co/api/v2/pokemon/${id}`);
        if (!response.ok) {
          alert("ポケモンが見つかりませんでした。");
          return;
        }
        this.pokemonData = await response.json();

        // ポケモンの分類（species）情報を取得
        const speciesResponse = await fetch(`https://pokeapi.co/api/v2/pokemon-species/${id}`);
        if (speciesResponse.ok) {
          this.speciesData = await speciesResponse.json();
        }
      } catch (error) {
        console.error("エラーが発生しました:", error);
        alert("データの取得中にエラーが発生しました。");
      }
    },
  },
};
</script>

<style lang="scss">
* {
  font-family: "DotGothic16", sans-serif;
  font-weight: 400;
  font-style: normal;
}

.pokemon-container {
  background: linear-gradient(180deg, #E70015 0%, #E70015 40%, #FFFFFF 40%, #FFFFFF 100%);
  border: 15px solid #000000;
  display: flex;
  justify-content: center;
  align-items: center;
  width: 95%;
  height: 850px;
  margin: 0 auto;


  .pokemon-area {
    text-align: center;
    align-items: center;
    background-color: #FFFFFF;
    border: 15px solid #000000;
    border-radius: 1000px;
    width: 750px;
    height: 750px;
    position: relative;
    margin: 0 auto;
  }

  &::before {
    content: "";
    position: absolute;
    top: 40%;
    width: 95%;
    height: 15px;
    background-color: black;
    transform: translateY(-50%);
  }

  .pokemon-name {
    font-size: 36px;
    padding-top: 110px;
  }

  .pokemon-image {
    padding-bottom: 15px;
  }

  .pokemon-detail {
    font-size: 24px;
    padding: 0 30px;
    margin: 0 auto;
    display: flex;
    align-items: center;
    justify-content: center;
    background-color: #EFEFEF;
    height: 130px;
    width: 500px;
  }

  .pokemon-property {
    .species {
      padding: 15px 0;
    }

    .type {
      padding-bottom: 15px;
    }

    .size {
      padding-bottom: 15px;
    }

    .ability {
      padding-bottom: 15px;
    }
  }
}
</style>
