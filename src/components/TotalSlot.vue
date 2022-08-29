<template>
  <h2 class="total">
    <!-- slot = élément pour recevoir un contenu -->
    <slot></slot> Tâches en attente
  </h2>
</template>

<script>
export default {
  name: 'TotalSlot'
}
</script>

<style scoped>
.total {
  color: red;
  margin: 1.5em 2px;
  font-size: 2.5em;
}
</style>