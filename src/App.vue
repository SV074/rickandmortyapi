<script setup>
import { ref, reactive,computed, onMounted } from 'vue';
import CardCharacter from './components/CardCharacter.vue';
import Button from './components/Button.vue';

const characters = ref([]);
const pages = ref([]);
const visibleButtons = ref([]);
const inputValueName = ref();
const inputValueStatus = ref();
const next = ref();
const prev = ref();

const page = ref({
  name: 2,
  id: '',
  isDisabled: false
});

const visiblePrev = computed(() => {
  if(prev.value) {
    return true;
  } else false;
})

const visibleNext = computed(() => {
  if(next.value) {
    return true;
  } else false;
})

function filterPages() {
  if(inputValueName.value || inputValueStatus.value)
  return characters.value = characters.value.filter(item => item.name === inputValueName.value || item.status === inputValueStatus.value);
}

function renderPage(item) {
  fetch(`https://rickandmortyapi.com/api/character/?page=${page.value.name}`)
    .then(response => response.json())
    .then(data => {
      characters.value = data.results;
      next.value = data.info.next;
      prev.value = data.info.prev;
    })
    .catch(error => {
      console.log(error);
    })
  
}

function renderPages() {
  fetch('https://rickandmortyapi.com/api/character')
    .then(response => response.json())
    .then(data => {
      characters.value = data.results;
      next.value = data.info.next;
      prev.value = data.info.prev;
      for (let index = 1; index <= data.info.pages; index++) {
        const pageNeed = {
          name: index,
          id: index,
          idDisabled: false
        }
        pages.value.push(pageNeed);
      }
      showVisibleButtons(data.info.prev, 3);
    })
    .catch(error => {
      console.log(error);
    })
}

function showVisibleButtons(firstIndex, lastIndex) {
  visibleButtons.value = pages.value.slice(firstIndex, lastIndex);
}

function firstPage() {
  page.value.name = '1';
  renderPage();
  showVisibleButtons(0, 3);
}

function lastPage() {
  page.value.name = '42';
  renderPage();
  showVisibleButtons(page.value.name - 3, page.value.name);
}

function goPage(item) {
  page.value = item;
  renderPage();
}

function previousPage() {
  if (page.value.name > 2) {
    page.value.name -= 1;
    renderPage();
    showVisibleButtons(page.value.name - 2, page.value.name + 1);
  }
}

function nextPage() {
  if (page.value.name < 41) {
    page.value.name += 1;
    renderPage();
    showVisibleButtons(page.value.name - 2, page.value.name + 1);
  }
}

onMounted(() => {
  renderPages();
})

</script>

<template>
  <div class="main">
    <div class="container">
      <div class="main__header">
        <div class="header-pagination">
          <div class="pagination">
            <div class="pagination__buttons">
              <button class="button-first" @click="firstPage" v-if="visiblePrev">
                << </button>
                  <button classs="button-previous" @click="previousPage" v-if="visiblePrev">
                    < </button>
            </div>
            <div class="pagination__pages">
              <Button v-for="item in visibleButtons" :numberPage="item" @click="goPage(item)"></Button>
            </div>
            <div class="pagination__buttons">
              <button class="button-next" @click="nextPage" v-if="visibleNext">
                >
              </button>
              <button classs="button-end" @click="lastPage" v-if="visibleNext">
                >>
              </button>
            </div>
          </div>
        </div>
        <div class="header-filtration">
          <div class="filtration-inputs">
            <input type="text" v-model="inputValueName" class="input-name" placeholder="Имя персонажа">
            <input type="text" v-model="inputValueStatus" class="input-status" placeholder="Статус персонажа">
          </div>
          <div class="button-apply">
            <button @click="filterPages">Применить</button>
          </div>
        </div>
      </div>
      <div class="main__list-characters">
        <div class="main__inner">
          <CardCharacter v-for="character in characters" :key="character.id" :character="character"></CardCharacter>
        </div>
      </div>
    </div>
  </div>
</template>

<style scoped>
.main {
  display: flex;
  flex-direction: column;
}

.main__header {
  padding: 20px;
}

.main__list-characters {
  background: rgb(39, 43, 51);
  width: 100%;
}

.main__inner {
  display: flex;
  flex-wrap: wrap;
  height: 100%;
}

.pagination {
  display: flex;
  justify-content: center;
}

.pagination__buttons {
  display: flex;
}

.header-filtration {
  margin-top: 20px;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
}
.filtration-inputs {
  display: flex;
  flex-direction: column;
}

.input-name, 
.input-status {
  margin: 10px;
  height: 25px;
  border-radius: 5px;
}

.red {
  color: red;
}
</style>
