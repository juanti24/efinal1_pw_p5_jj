<template>
  <div class="container" v-if="puntaje < 10 && intento < 5">
    <h1>Casino</h1>
    <div class="score">
      <h2>Puntaje: {{ puntaje }}</h2>
      <h2>Intentos: {{ intento }}</h2>
    </div>
    <div class="imagenes">
      <PokemonImg :nombre="nombre1" :mostrar="dir1" />
      <PokemonImg :nombre="nombre2" :mostrar="dir2" />
      <PokemonImg :nombre="nombre3" :mostrar="dir3" />
    </div>
  </div>

  <button @:click="obtenerValores()" v-if="puntaje < 10 && intento < 5">
    Jugar
  </button>
  <div class="win-message" v-if="puntaje >= 10 && intento <= 5">
    <h2>Puntaje: {{ puntaje }}</h2>
    <h2>Felicitaciones has ganado un premio de $10.000,00</h2>
    <button @:click="reiniciarJuego">Nuevo Juego</button>
  </div>
  <div class="lose-message" v-if="puntaje < 10 && intento >= 5">
    <h2>Has utilizado tus 5 intentos</h2>
    <h2>El juego ha terminado, intentalo nuevamente</h2>
    <button @:click="reiniciarJuego">Nuevo Juego</button>
  </div>
</template>
  
  <script>
import PokemonImg from "../components/PokemonImg.vue";

export default {
  name: "CasinoPage",
  data() {
    return {
      nombre1: "XXXXXXXXXXX",
      nombre2: "XXXXXXXXXXX",
      nombre3: "XXXXXXXXXXX",
      puntaje: 0,
      intento: 0,
      dir1: "https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/other/dream-world/1.svg",
      dir2: "https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/other/dream-world/2.svg",
      dir3: "https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/other/dream-world/3.svg",
    };
  },
  components: {
    PokemonImg,
  },
  methods: {
    async consumirAPI() {
      const data = await fetch("https://yesno.wtf/api").then((r) => r.json());

      return data;
    },

    async repartir() {
      const miVector = [];
      for (let i = 0; i < 3; i++) {
        const valores = await this.consumirAPI();

        const pok = {
          respuesta: valores.answer,
          foto: valores.image,
        };
        miVector.push(valores);
      }
      console.table(miVector);
      return miVector;
    },

    async obtenerValores() {
      const res = await this.repartir();
      const pk1 = res[0];
      const pk2 = res[1];
      const pk3 = res[2];

      this.dir1 = pk1.image;
      this.dir2 = pk2.image;
      this.dir3 = pk3.image;

      this.nombre1 = pk1.answer;
      this.nombre2 = pk2.answer;
      this.nombre3 = pk3.answer;

      this.calcularPuntajeIntentos();
    },

    calcularPuntajeIntentos() {
      if (this.nombre1 == this.nombre2 && this.nombre2 == this.nombre3) {
        this.puntaje += 5;
        this.intento++;
      } else if (
        this.nombre1 == this.nombre2 ||
        this.nombre2 == this.nombre3 ||
        this.nombre1 == this.nombre3
      ) {
        this.puntaje += 2;
        this.intento++;
      } else {
        this.intento++;
      }
    },

    reiniciarJuego() {
      this.nombre1 = "XXXXXXXXXXX";
      this.nombre2 = "XXXXXXXXXXX";
      this.nombre3 = "XXXXXXXXXXX";
      this.puntaje = 0;
      this.intento = 0;
      this.dir1 =
        "https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/other/dream-world/1.svg";
      this.dir2 =
        "https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/other/dream-world/2.svg";
      this.dir3 =
        "https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/other/dream-world/3.svg";
    },
  },
};
</script>
  
  <style>
.imagenes {
  display: flex;
  justify-content: center;
  align-items: center;
  flex-direction: row;
}

img {
  margin-left: 10px;
  margin-right: 10px;
}

.win-message h2 {
  color: blue;
}

.win-message button {
  border: 2px solid black;
  width: 100px;
  height: 30px;
}

.lose-message h2 {
  color: red;
}

.lose-message button {
  border: 2px solid black;
  width: 100px;
  height: 30px;
}

.score {
  display: flex;
  align-items: center;
  justify-content: center;
  flex-direction: row;
}

.score h2 {
  margin-right: 10px;
  margin-left: 10px;
}
</style>