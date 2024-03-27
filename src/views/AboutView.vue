<template>
  <!-- Novo botão "Tirar Dúvidas" -->
  <div class="duvidas">
    <button @click="toggleHelp" class="btn btn-secondary" title="Tirar Dúvidas">Tirar Dúvidas</button>

    <!-- Área de texto para exibir quando "Tirar Dúvidas" for clicado -->
    <p v-if="showHelp">
      A alteração do RGB em uma imagem ocorre manipulando os valores dos canais de cor vermelho (R), verde (G) e azul
      (B)
      de cada pixel da imagem. Cada pixel em uma imagem digital é representado por uma combinação de intensidades de cor
      nos canais RGB, onde valores mais altos representam uma maior intensidade da cor correspondente e valores mais
      baixos representam uma menor intensidade.

      Por exemplo, se tivermos um pixel com a cor RGB(10, 10, 10) e aplicarmos um aumento de intensidade para RGB(20,
      20,
      20), isso resultará em uma alteração perceptível na cor do pixel. Aumentar os valores de R, G e B resultará em uma
      cor mais clara e mais brilhante, enquanto diminuir esses valores resultará em uma cor mais escura e mais opaca.

      Essencialmente, ao alterar os valores RGB de uma imagem, estamos ajustando a intensidade das cores vermelha, verde
      e
      azul em cada pixel, o que modifica a cor geral da imagem. Isso é feito de forma individual para cada pixel,
      permitindo uma ampla variedade de manipulações de cor e efeitos visuais em uma imagem digital.</p>
  </div>
  <div class="editor-area">
    <div class="container-rgb">
      <input type="file" @change="handleFileUpload" class="input-file">
      <div class="input-group">
        <label for="red" class="label">Red:</label>
        <input type="number" id="red" v-model="red" class="input" min="0" max="255">
      </div>
      <div class="input-group">
        <label for="green" class="label">Green:</label>
        <input type="number" id="green" v-model="green" class="input" min="0" max="255">
      </div>
      <div class="input-group">
        <label for="blue" class="label">Blue:</label>
        <input type="number" id="blue" v-model="blue" class="input" min="0" max="255">
      </div>
      <button @click="uploadImage" type="button" class="btn btn-primary" title="Aplicar e Visualizar">Aplicar e
        Visualizar</button>
      <button @click="downloadImage" type="button" class="btn btn-outline-primary" title="Baixar Imagem">Baixar
        Imagem</button>


    </div>
    <div v-if="imageUrl" class="image-preview">
      <img :src="imageUrl" alt="Imagem Recebida" class="preview">
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      file: null,
      imageUrl: null,
      red: 0,
      green: 0,
      blue: 0,
      showHelp: false // Variável de estado para controlar a exibição do texto de ajuda
    };
  },
  methods: {
    handleFileUpload(event) {
      this.file = event.target.files[0];
    },
    async uploadImage() {
      const formData = new FormData();
      formData.append('image', this.file);
      formData.append('red', this.red);
      formData.append('green', this.green);
      formData.append('blue', this.blue);

      try {
        const response = await fetch('https://backend-image-edit-1.onrender.com/upload', {
          method: 'POST',
          body: formData
        });
        const data = await response.blob();
        const imageUrl = URL.createObjectURL(data);
        this.imageUrl = imageUrl;
      } catch (error) {
        console.error('Erro ao enviar imagem:', error);
      }
    },
    downloadImage() {
      const link = document.createElement('a');
      link.href = this.imageUrl;
      link.download = 'edited_image.png';
      document.body.appendChild(link);
      link.click();
      document.body.removeChild(link);
    },
    toggleHelp() {
      this.showHelp = !this.showHelp; // Alternar o estado de exibição do texto de ajuda
    }
  }
};
</script>

<style>
.editor-area {
  width: 100%;
  display: flex;
  margin-top: 10%;
}

.container-rgb {
  margin-left: 16px;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  height: 300px;
  width: 400px;
  border: 1px solid #ccc;
}

.input-group {
  margin-bottom: 10px;
  display: flex;
  align-items: center;
}

.label {
  width: 70px;
  text-align: right;
  margin-right: 10px;
}

.input {
  padding: 5px;
  border: 1px solid #ccc;
  border-radius: 5px;
  min-width: 270px;
}

.input-file {
  margin-bottom: 10px;
  width: 330px;
}

.image-preview {
  max-width: 300px;
  max-height: 300px;
}

.preview {
  max-width: 300px;
  max-height: 300px;
  margin-left: 16px;
}

.btn-primary {
  width: 325px;
  margin-top: 10px;
}

.btn-outline-primary {
  width: 325px;
  margin-top: 10px;
}

/* Estilo para o novo botão "Tirar Dúvidas" */
.btn-secondary {
  width: 325px;
  margin-top: 10px;
}

.duvidas {
  padding-left: 8px;
}

/* Estilo para o texto de ajuda */
p {
  margin-top: 10px;
}
</style>
