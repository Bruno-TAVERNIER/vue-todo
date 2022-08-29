<template>
  <h1>My Todo List</h1>
  <!-- <h2> {{ todos.length }} Tâches en attente</h2>-->
  <!-- on remplace par un Component de type Slot qui sert juste à l'affichage -->
  <TotalSlot>{{ todos.length }}</TotalSlot>
  <ul>    
    <!-- un component reçoit des valeurs à traiter via des paramètres (props)
        et répondra en émettant des évènements que le parent devra traiter localement -->
    <TodoComponent v-for="todo in todos" :key="todo.id" :todo="todo" @started="started" @ended="ended" />
  </ul>
  <!-- sauvegarde dans le Ls pour ne pas perdre les modifications -->
  <button @click="sauve">Sauvegarder (LS)</button>
</template>

<script>

import TotalSlot from './components/TotalSlot.vue';
import TodoComponent from './components/TodoComponent.vue';

export default {
    name: "App",
    data() {
        return {
            todos: [
                { id: 1, nom: "Cours Vue", desc: "Commencer le cours Vue", statut: "started" },
                { id: 2, nom: "Exercice Vue", desc: "Exercice Todo", statut: "waiting" },
                { id: 3, nom: "Exercice todo V2", desc: "todo amélioré", statut: "waiting" },
                { id: 4, nom: "Appel M2I Sign", desc: "Faire l'appel", statut: "ended" },
                { id: 5, nom: "Slot", desc: "Ajouter un slot", statut: 'started'}
            ]
        };
    },
    methods: {
        /* fonction exécutée suite à un "event" de l'élément enfant TodoComponent */
        started(id) {
            console.log(id);
            this.todos.forEach(function (todo) {
                if (todo.id == id)
                    todo.statut = "started";
            });
        },
        /* idem fonction suite à event de TodoComponent */
        ended(id) {
            console.log(id);
            this.todos.forEach(function (todo) {
                if (todo.id == id)
                    todo.statut = "ended";
            });
        },
        //enregistrement dans le LS de la liste de todos
        sauve() {
            localStorage.setItem("myTodo", JSON.stringify(this.todos));
        }
    },
    //hook de Vue => document.ready avec JQuery
    mounted() {
        // si le LS n'est pas vide
        if (localStorage.getItem("myTodo")) {
            let myTodo = localStorage.getItem("myTodo");
            this.todos = JSON.parse(myTodo);
        }
    },
    components: { TotalSlot, TodoComponent }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
ul {
  list-style-type: none;
}

</style>
