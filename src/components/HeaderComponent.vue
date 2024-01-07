<script setup lang="ts">
const props = defineProps<{
  userScore: number,
  computerScore: number,
}>()
</script>

<template>
  <div class="header">
    <div class="title">
    </div>
    <div class="score">
      <span>Score</span>
      <h1>{{ props.userScore }}</h1>
<!--      <span>Computer Score: {{ props.computerScore }}</span>-->
    </div>
  </div>
</template>

<style scoped>

</style>