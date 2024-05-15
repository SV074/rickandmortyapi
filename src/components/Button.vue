<script setup>
import { ref } from 'vue';

const props = defineProps({
    numberPage: Object
})

</script>

<template>
<button :class="{ red: numberPage.isDisabled }">
  {{ numberPage.name }}
</button>



</template>

<style scoped>
.red {
    color: red;
}
</style>