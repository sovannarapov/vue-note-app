<template>
  <main>
    <div v-if="showModal" class="overlay">
      <div class="modal">
        <textarea v-model.trim="newNote" name="note" id="note" cols="30" rows="10"></textarea>
        <p v-if="errorMessage">{{ errorMessage }}</p>
        <button @click="addNote">Add</button>
        <button class="close" @click="onCloseModal">Close</button>
      </div>
    </div>
    <div class="container">
      <header>
        <h1>Notes</h1>
        <button @click="onShowModal">+</button>
      </header>
      <div class="cards-container">
        <div
          v-for="note in notes"
          :key="note.id"
          class="card"
          :style="{ backgroundColor: note.backgroundColor}"
        >
          <p class="main-text">{{ note.text }}</p>
          <p class="date">{{note.date.toLocaleDateString('en-US')}}</p>
        </div>
      </div>
    </div>
  </main>
</template>

<script setup>
import { isEmpty } from 'lodash';
import { ref } from 'vue';

const showModal = ref(false);
const newNote = ref('');
const notes = ref([]);
const errorMessage = ref('');

const onShowModal = () => {
  showModal.value = true;
}

const onCloseModal = () => {
  showModal.value = false;
  errorMessage.value = '';
}

function getRandomColor() {
  return 'hsl(' + Math.random() * 360 + ', 100%, 75%)';
}

const addNote = () => {
  if (isEmpty(newNote.value)) {
    return errorMessage.value = 'Please enter something to add a note!'
  }
  notes.value.push({
    id: Math.floor(Math.random() * 1000000),
    text: newNote.value,
    date: new Date(),
    backgroundColor: getRandomColor(),
  });
  showModal.value = false;
  newNote.value = '';
  errorMessage.value = '';
}
</script>

<style scoped>
main {
  width: 100vw;
  height: 100vh;
}

.container {
  max-width: 1000px;
  padding: 10px;
  margin: 0 auto;
}

header {
  display: flex;
  justify-content: space-between;
  align-items: center;
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
  background-color: black;
  border-radius: 100%;
  color: white;
  font-size: 20px;
}

.card {
  width: 275px;
  height: 275px;
  background-color: rgb(237, 182, 44);
  padding: 10px;
  border-radius: 15px;
  display: flex;
  flex-direction: column;
  justify-content: space-between;
  margin-right: 20px;
  margin-bottom: 20px;
}

.date {
  font-size: 12.5px;
  font-weight: bold;
}

.cards-container {
  display: flex;
  flex-wrap: wrap;
}

.overlay {
  position: absolute;
  width: 100%;
  height: 100%;
  background-color: rgba(0,0,0,0.77);
  z-index: 10;
  display: flex;
  align-items: center;
  justify-content: center;
}

.modal {
  width: 750px;
  background-color: white;
  border-radius: 10px;
  padding: 30px;
  position: relative;
  display: flex;
  flex-direction: column;
}

.modal button {
  padding: 10px 20px;
  font-size: 20px;
  width: 100%;
  background-color: blueviolet;
  border: none;
  color: white;
  cursor: pointer;
  margin-top: 15px;
}

.modal .close {
  background-color: rgb(193, 15, 15);
  margin-top: 7px;
}

.modal p {
  color: rgb(193, 15, 15);
}
</style>