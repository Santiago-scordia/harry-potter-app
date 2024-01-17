<template>
  <div>
    <input v-model="characterName" @input="fetchCharacterImage" class="search-input" placeholder="Buscar personaje" />
    <div v-if="characterData" class="character-info">
      <p>{{ characterData.name }}</p>
      <p>Casa: {{ characterData.house }}</p>
      <p>Edad: {{ 2023 - characterData.yearOfBirth }}</p>
      <img :src="characterData.image" alt="Imagen del personaje" class="character-image" />
    </div>
    <p v-if="error" class="error-message">{{ error }}</p>
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
          this.characterData = null;
          this.error = '';
          return;
        }

        const character = data.find(item => item.name.toLowerCase().includes(this.characterName.toLowerCase()));

        if (character) {
          this.characterData = character;
          this.error = '';
        } else {
          this.characterData = null;
          this.error = 'Personaje no encontrado';
        }
      } catch (error) {
        this.characterData = null;
        this.error = 'Error al obtener la imagen del personaje';
      }
    },
  },
};
</script>


<style scoped>
.search-input {
  padding: 8px;
  margin-bottom: 16px;
  font-size: 16px;
  border: 1px solid #ccc;
  border-radius: 4px;
  width: 300px;
}

.character-info {
  margin-top: 16px;
}

.character-info p {
  font-size: 18px;
  margin-bottom: 8px;
}

.character-image {
  width: 100%;
  max-width: 300px;
  height: auto;
  border-radius: 4px;
}

.error-message {
  color: red;
  margin-top: 8px;
}
</style>
