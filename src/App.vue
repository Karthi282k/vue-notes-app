<template>
  <main>
    <div class="overlay" v-show="showModal">
      <div claas="modal">
        <textarea name="note" v-model.trim="newNote" id="note" cols="90" rows="10"></textarea>
        <p v-if="errorMessage" class="err-p">{{ errorMessage }}</p>
        <button class="addbtn" @click="addNote">Add Note</button>
        <button class="clsbtn" @click="showModal = false">Close</button>
      </div>
    </div>
    <div class="container">
      <header>
        <h1>Notes</h1>
        <button @click="showModal = true">+</button>
      </header>
      <div class="cards-container">
        <div v-for="note in notes" class="card" :style="{ backgroundColor: note.backgroundColor }" :key="note.id">
          <p class="main-text" style="overflow-wrap: break-word;">{{ note.text }}</p>
          <p class="date">{{ note.date.toLocaleDateString("en-US") }} <button @click="deltNotes(note.id)"
              class="dlt">Delete</button></p>
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
  background-color: rgb(21, 20, 20);
  border-radius: 100%;
  color: white;
  font-size: 20px;
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
  background-color: rgb(0, 0, 0, 0.77);
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

.addbtn {
  padding: 10px 20px;
  font-size: 20px;
  width: 75%;
  background-color: blueviolet;
  border: none;
  color: white;
  cursor: pointer;
  margin-top: 10px;

}

.clsbtn {
  padding: 10px 20px;
  font-size: 20px;
  width: 75%;
  background-color: violet;
  border: none;
  color: white;
  cursor: pointer;
  margin-top: 7px;
}

.err-p {
  color: red;
}

.dlt {
  float: right;
  background-color: blueviolet;
  color: white;
  padding: 2.5px 2.5px;
  border: none;
  cursor: pointer;
}
</style>

<script setup>
import { ref } from 'vue';

const showModal = ref(false);
const newNote = ref("");
const errorMessage = ref("");
const notes = ref([]);
function getRandomColor() {
  const color = "hsl(" + Math.random() * 360 + ",100%,75%";
  return color
}
function addNote() {
  if (newNote.value.length < 10) {
    return errorMessage.value = "Note needs to be 10 characters or more";
  }
  notes.value.push({
    id: Math.floor(Math.random() * 100000),
    text: newNote.value,
    date: new Date(),
    backgroundColor: getRandomColor()
  });
  showModal.value = false;
  newNote.value = "";
  errorMessage.value = "";

}
function deltNotes(uq_id) {
  Object.entries(notes.value).forEach(([key, value]) => {
   // console.log('working 1');
    // console.log('for id '+notes.value[key].id+' given id '+uq_id);
    if (notes.value[key].id == uq_id) {
      console.log('working 2')
      notes.value.splice(key, 1);
      return;
    }
  });
}

</script>