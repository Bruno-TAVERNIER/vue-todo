<template>
  <li :class="todo.statut">
    <NomSlot>{{ todo.nom }}</NomSlot>
    <h4>{{ todo.desc }}</h4>
    <h5>{{ todo.statut }}</h5>
    <!-- bouton pour démarrer (new statut started) une tâche uniquement si statut=waiting -->
    <button v-show="todo.statut=='waiting'" @click="start">Démarrer</button>
    <!-- bouton pour arrêter (new statut ended) une tâche uniquement si statut=started -->
    <button v-show="todo.statut=='started'" @click="end">Arrêter</button>
  </li>
</template>

<script>
import NomSlot from './NomSlot.vue';
  export default {
    name: 'TodoComponent',
    /* les propriétés viennent de l'élément parent */
    props: {
      todo: Object
    },
    methods: {
      start() {
        // émission d'un event personnalisé vers le parent
        this.$emit('started', this.todo.id);
      },
      end() {
        // émission d'un évent personnalisé vers le parent
        this.$emit('ended', this.todo.id);
      }
    },
    components: { NomSlot }
  }
</script>

<style scoped>
li { 
  width: 85%;
  margin: 0 auto;
  border: 1px solid silver;
  margin-bottom: 5px;
  padding: 10px;
}
.waiting {
  background-color: lime;
}
.started {
  background-color: orange;
}
.ended {
  background-color: red;
}
</style>