<script setup>
import { provide, ref } from 'vue';
import LeftLayout from './components/leftLayout.vue';
import RightLayout from './components/rightLayout.vue';
import CompleteLayout from './components/completeLayout.vue';

const cardName = ref('Jane Appleseed')
const cardNumbers = ref('0000000000000000')
const cardMonth = ref('00')
const cardYear = ref('00')
const cardCvv = ref('000')
const isVisible = ref(true)
provide('inputDetails', {
  cardName,
  cardNumbers,
  cardMonth,
  cardYear,
  cardCvv,
  isVisible
})
</script>

<template>
  <main>
    <LeftLayout />
    <RightLayout v-show="isVisible" />
    <CompleteLayout v-show="!isVisible" />
  </main>
</template>

<style lang="scss" scoped>
main {
  display: flex;
  justify-content: space-between;
  align-items: center;

}

@media (max-width: 568px) {
  main {
    flex-direction: column;
  }
}
</style>
