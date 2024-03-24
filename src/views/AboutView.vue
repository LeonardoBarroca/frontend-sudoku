<template>
  <div>
    <input type="file" @change="handleFileUpload">
    <button @click="uploadImage">Enviar Imagem</button>
    <div v-if="imageUrl">
      <img :src="imageUrl" alt="Imagem Recebida">
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      file: null,
      imageUrl: null
    };
  },
  methods: {
    handleFileUpload(event) {
      this.file = event.target.files[0];
    },
    async uploadImage() {
      const formData = new FormData();
      formData.append('image', this.file);

      try {
        const response = await fetch('https://backend-image-edit-1.onrender.com/upload', {
          method: 'POST',
          body: formData
        });
        const data = await response.blob(); // Retorna os dados da imagem como um blob
        const imageUrl = URL.createObjectURL(data); // Cria uma URL para o blob
        this.imageUrl = imageUrl; // Define a URL da imagem para exibição
      } catch (error) {
        console.error('Erro ao enviar imagem:', error);
      }
    }
  }
};
</script>
