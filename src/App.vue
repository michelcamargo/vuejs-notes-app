<script setup>
  import { ref } from "vue";

  const showModal = ref(false);
  const newNote = ref('');
  const notes = ref([]);
  const errorMessage = ref('');
  const addNote = () => {
    if (newNote.value.length < 10) return errorMessage.value = 'A anotação precisa ter ao menos 10 caracteres';

    notes.value.push({
      id: Math.floor(Math.random() * 1000000),
      text: newNote.value,
      date: new Date(),
      backgroundColor: "hsl(" + Math.random() * 360 + ", 100%, 75%",
    })
    showModal.value = false;
    newNote.value = '';
    if (errorMessage) errorMessage.value = '';
  }

</script>

<template>
  <main>
    <div v-if="showModal" class="overlay">
      <div class="modal">
        <textarea v-model.trim="newNote" name="note" id="note" cols="30" rows="10" />
        <p class="error-message" v-if="errorMessage">{{ errorMessage }}</p>
        <div class="action-buttons">
<!--          <button class="add-note" @click="addNote" :style="{ opacity: isAbleToAdd ? 1 : .3 }">Criar nota</button>-->
          <button class="add-note" @click="addNote">Criar nota</button>
          <button class="close-modal" @click="showModal = false">Fechar</button>
        </div>
      </div>
    </div>
    <div class="container">
      <header>
        <h1>Anotações</h1>
        <button @click="showModal = true">+</button>
      </header>
      <div class="cards-container">
        <div class="card"
            v-for="note in notes"
            :key="note.id"
            :style="{ backgroundColor: note.backgroundColor }"
        >
          <p class="main-text">{{ note.text }}</p>
          <p class="date">{{ note.date.toLocaleDateString('pt-BR') }}</p>
        </div>
      </div>
    </div>
  </main>
</template>

<style scoped>
  main {
    height: 100vh;
    width: 100vw;
  }

  header {
    display: flex;
    justify-content: space-between;
    align-items: center;
  }

  .container {
    max-width: 1000px;
    padding: 10px;
    margin: 0 auto;
  }

  h1 {
    font-weight: bold;
    margin-bottom: 25px;
    font-size: 75px;
  }

  header button {
    border: none;
    padding: 10px;
    width: 50px;
    height: 50px;
    cursor: pointer;
    background: white;
    border-radius: 100%;
    color: rgb(21,20,20);
    font-size: 20px;
  }

  .cards-container {
    display: flex;
    flex-wrap: wrap;
    column-gap: 20px;
    row-gap: 20px;
  }

  .card {
    width: 225px;
    height: 225px;
    background-color: rgb(237, 182, 44);
    padding: 10px;
    border-radius: 15px;
    display: flex;
    flex-direction: column;
    justify-content: space-between;
  }

  .main-text {
    overflow-y: auto;
    color: rgb(27, 40, 35);
  }

  .date {
    font-weight: bold;
    font-size: 12px;
    color: rgb(27, 40, 35);
  }

  .overlay {
    position: absolute;
    width: 100%;
    height: 100%;
    background-color: rgba(0,0,0,0.7);
    z-index: 10;
    display: flex;
    align-items: center;
    justify-content: center;
  }

  .modal {
    width: 35%;
    background-color: white;
    border-radius: 10px;
    padding: 30px;
    position: relative;
    display: flex;
    flex-direction: column;
  }

  .modal button {
    padding-top: 10px;
    padding-bottom: 10px;
    font-size: 20px;
    border: none;
    color: white;
    cursor: pointer;
  }

  .action-buttons {
    width: 100%;
    margin-top: 15px;
    display: flex;
    flex-direction: row;
    column-gap: 20px;
  }

  .add-note {
    width: 750px;
    background-color: blueviolet;
  }

  .close-modal {
    width: fit-content;
    padding-left: 50px;
    padding-right: 50px;
    background-color: rgb(193, 15, 15);
  }

  .error-message {
    color: rgb(193, 15, 15);
  }

</style>
