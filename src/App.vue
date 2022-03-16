<template>
  <div id="app">
    <div class="container">
      <div class="row">
        <div class="col-12">
          <img
            src="https://images-wixmp-ed30a86b8c4ca887773594c2.wixmp.com/i/87044f58-c765-43c5-bc51-8613e3ac7ab1/ddew4m7-c69a2c41-518f-48ca-ba35-8ab1895464e0.png"
            alt="pokemon logo"
            class="logo"
          />
          <h1>{{ setTitle }}</h1>
        </div>
        <div class="col-12 my-4 d-flex">
          <input type="text" class="form-control mx-4" v-model="mySearch" />
          <button class="btn btn-secondary my-2" @click="search()">
            Buscar
          </button>
        </div>
      </div>
      <div class="row">
        <div class="col-12">
          <img :src="imgSrc" alt="" v-if="imgSrc" />
        </div>
        <div v-if="vshow">
          <div class="col-12">
            <h2 class="my-4">Habilidades</h2>
            <ul class="list-group">
              <li
                class="list-group-item"
                v-for="(ability, i) in abilities"
                :key="i"
              >
                {{ ability }}
              </li>
            </ul>
          </div>
          <div class="col-12">
            <h2 class="my-4">Movimientos</h2>
            <ul class="row">
              <li
                class="col-4 list-group-item"
                v-for="(move, i) in moves"
                :key="i"
              >
                {{ move }}
              </li>
            </ul>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "App",
  data() {
    return {
      titulo: "Pokeguía",
      myData: {},
      mySearch: "",
      URL: "https://pokeapi.co/api/v2/pokemon/",
      vshow: true,
    };
  },
  created() {
    this.getData("pikachu");
  },
  computed: {
    setTitle() {
      return this.titulo.toUpperCase();
    },
    imgSrc() {
      return this.myData?.sprites?.other?.dream_world?.front_default;
    },
    abilities() {
      return this.myData?.abilities?.map((a) => a.ability.name);
    },
    moves() {
      return this.myData?.moves?.map((a) => a.move.name);
    },
  },
  methods: {
    async getData(pokemon) {
      try {
        const req = await fetch(`${this.URL}${pokemon}`);
        const data = await req.json();
        this.myData = data;
        this.vshow = true;
      } catch (error) {
        this.vshow = false;
        this.myData.sprites.other.dream_world.front_default =
          // eslint-disable-next-line
          "data:image/jpeg;base64,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";
        this.myData.abilities = [];
        this.myData.moves = [];
        this.titulo = "Pokemon no encontrado";
        console.log(error);
      }
    },
    search() {
      this.titulo = this.mySearch;
      console.log(this.mySearch);
      this.getData(this.mySearch.toLowerCase());
      console.log(this.myData);
    },
  },
};
</script>

<style lang="scss">
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
.logo {
  width: 300px;
}
</style>
