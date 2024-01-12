<template>
  <div class="container">
    <div class="imagen">
      <img :src="img" alt="No su pudo mostrar la imagen" />

      <label for="">{{ nombre }}</label>
    </div>
  </div>
</template>
  
  <script>
export default {
  name: "PokemonImg",

  props: {
    nombre: {
      type: String,
      required: true,
    },
    mostrar: {
      type: String,
      required: true,
    },
  },

  data() {
    return {
      img: this.mostrar,
    };
  },
};
</script>
  
  <style scoped>
.imagen {
  display: flex;
  align-items: center;
  justify-content: center;
  flex-direction: column;
}

img {
  width: 400px;
  height: 300px;
}
</style>