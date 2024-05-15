<script setup>
import { computed } from 'vue';

const props = defineProps({
  character: Object
})

const iconStatusClass = computed(() => {
  if(props.character.status === 'Alive') {
    return 'red-background';
  } else if(props.character.status === 'Dead') {
    return 'green-background';
  }
})

</script>

<template>
  <div class="card">
    <div class="card__img">
      <img :src=character.image alt="">
    </div>  
    <div class="card__character-info">
      <div class="card__section">
        <div class="card__character-name"><h2>{{ character.name }}</h2></div>
      <div class="card__character-species-status">
        <span class="status-icon" :class="iconStatusClass"></span>
         <span>{{ character.status }} - {{ character.species }}</span>
      </div>
      </div>
      <div class="card__section">
        <span class="text-gray">Last known location:</span>
        <div class="card__character-location-title-name">{{ character.location.name }}</div>
      </div>
      <div class="card__section">
        <span class="text-gray">First seen in:</span>
        <div class="card__character-first-seen-title-name">{{ character.origin.name }}</div>
      </div>
    </div>
  </div>
</template>

<style scoped>
.card {
  width: 600px;
  height: 220px;
  display: flex;
  overflow: hidden;
  background: rgb(60, 62, 68);
  border-radius: 0.5rem;
  margin: 0.75rem;
  box-shadow: rgba(0, 0, 0, 0.1) 0px 4px 6px -1px, rgba(0, 0, 0, 0.06) 0px 2px 4px -1px;
}

.card__img {

}

.card__character-info {
  flex: 3 1 0%;  
  display: flex;
  flex-direction: column;
  color: rgb(255, 255, 255);
  padding: 0.75rem;
}

.card__section {
  flex: 1 1 0%;   
  display: flex;
  flex-direction: column;
}

.card__character-name {
  font-size: 1.5rem;
}

.card h2 {
  margin: 0;
  padding: 0;
}

.card__character-species-status {
  display: flex;
  align-items: center;
}

.card span {
  font-size: 16px;
  font-weight: 500;
}

.text-gray {
  color: rgb(158, 158, 158);
}

.status-icon {
  display: flex;
  height: 0.5rem;
  width: 0.5rem;
  margin-right: 0.375rem;
  border-radius: 50%;
  background: rgb(158, 158, 158);
}

.red-background {
  background: rgb(214, 61, 46);
}

.green-background {
  background: rgb(85, 204, 68);
}

.grey-background {
  
}
</style>
