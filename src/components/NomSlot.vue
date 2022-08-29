<template>
  <div class="nom">
    <slot></slot>
  </div>
</template>

<script>
  // Un slot sert uniquement à de l'affichage (réutilisation)
  export default {
    name: 'NomSlot'
  }
</script>

<style scoped>
  .nom {
    font-size: 5em;
    color: black;
    text-shadow: 3px 3px 3px silver;
    margin: 5px;
  }
</style>