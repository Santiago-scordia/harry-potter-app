<template>
  <div>
    <input v-model="characterName" @input="fetchCharacterImage" placeholder="Nombre del personaje" />
    <div v-if="characterData">
      <p>{{ characterData.name }}</p>
      <img :src="characterData.image" alt="Imagen del personaje" />
    </div>
    <p v-if="error">{{ error }}</p>
  </div>
</template>

<script>
export default {
  data() {
    return {
      characterName: '',
      characterData: null,
      error: '',
    };
  },
  methods: {
    async fetchCharacterImage() {
      try {
        const response = await fetch(`https://hp-api.onrender.com/api/characters`);
        const data = await response.json();

        if (this.characterName.trim() === '') {
          this.characterData = null; // Limpiar los datos del personaje si el nombre está vacío
          this.error = ''; // Limpiar el mensaje de error
          return;
        }

        // Encuentra el objeto correspondiente al personaje ingresado
        const character = data.find(item => item.name.toLowerCase().includes(this.characterName.toLowerCase()));

        if (character) {
          this.characterData = character;
          this.error = ''; // Limpia el mensaje de error si se encuentra el personaje
        } else {
          this.characterData = null; // Limpia los datos del personaje si no se encuentra
          this.error = 'Personaje no encontrado'; // Mensaje de error
        }
      } catch (error) {
        this.characterData = null; // Limpia los datos del personaje en caso de error
        this.error = 'Error al obtener la imagen del personaje'; // Mensaje de error
      }
    },
  },
};
</script>

<style scoped>
.container{
  background-color: #42b983;
}

</style>
