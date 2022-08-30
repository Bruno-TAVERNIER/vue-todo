<template>
  <h1 id="titre" ref="titre">My Todo List</h1>
  <!-- <h2> {{ todos.length }} Tâches en attente</h2>-->
  <!-- on remplace par un Component de type Slot qui sert juste à l'affichage -->
  <!--<TotalSlot>{{ todos.length }}</TotalSlot>-->
  <TotalSlot>{{ nbTodos }}</TotalSlot>
  <!-- interpolation du résultat d'une fonction -->
  <h3>{{ hasTodos }}</h3>
  <ul>    
    <!-- un component reçoit des valeurs à traiter via des paramètres (props)
        et répondra en émettant des évènements que le parent devra traiter localement -->
    <TodoComponent v-for="todo in todos" :key="todo.id" :todo="todo" @started="started" @ended="ended" />
  </ul>
  <div>
    <!-- v-model => liaison bidirectionnelle entre la variable et le DOM 
        .lazy détection du change (ou blur) plutot que modif
        .trim supprime les espaces avant et après
        .number pour des valeurs numériques uniquement -->
    <!--<input type="text" v-model.lazy="input1" placeholder="votre nom"/>
    <input type="number" v-model.number="input2" placeholder="votre age"/>
    <p> {{ input1 }} {{ input2 }}</p>-->
    <label>Nouvelle tâche</label>
    <input type="text" v-model="newTodo" /><br/>
    <label>Description</label>
    <textarea v-model="descTodo" /><br/>
    <button @click="addTodo">Ajouter la tâche</button>
  </div>
  <!--<div>
    <select v-model="sel1">
        <option disabled selected value="0">Choisissez une valeur</option>
        <option value="1">A</option>
        <option value="2">B</option>
        <option value="3">C</option>
        <option value="4">D</option>
    </select>
    <p>{{ sel1 }}</p>
  </div>-->
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
            ],
            input1: 'Azerty',
            sel1: 0,
            input2: 0,
            newTodo: '',
            descTodo: ''
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
        },
        addTodo() {
            //récup last id
            let id = this.todos.length + 1; // on a démarré avec ID = 1
            //création objet todo
            let todo = {id: id, nom: this.newTodo, desc: this.descTodo, statut: 'waiting'};
            //push dans todos
            this.todos.push(todo);
            //remise à blanc du formulaire
            this.newTodo = '';
            this.descTodo = '';
        }
    },
    computed: {
        //fonctions mise en cache, sans paramètres et obligatoirement valeur de retour
        nbTodos() {
            return this.todos.length;
        },
        hasTodos() {
            return this.todos.length > 0 ? 'Oui': 'Non';
        }
    },
    /* surveillance des modifications dans "data" */
    watch: {
        // variable à surveiller, premier param nouvelle valeur, second param ancienne valeur 
        todos(newVal, oldVal) {
            console.log('avant ' + oldVal);
            console.log('apres ' + newVal);
        },
        /*newTodo(newVal2, oldVal2) {
            if(newVal2.length < oldVal2.length) this.newTodo = oldVal2;
        }*/
    },
    //hook de Vue => document.ready avec JQuery
    mounted() {
        // si le LS n'est pas vide
        if (localStorage.getItem("myTodo")) {
            let myTodo = localStorage.getItem("myTodo");
            this.todos = JSON.parse(myTodo);
        }
        let titre = document.getElementById('titre');
        console.log(titre.textContent);
        // les éléments avec un attribut ref
        console.log(this.$refs);
        console.log(this.$refs.titre.textContent);
    },
    beforeCreate() {
        console.log('initialisation du composant');
    },
    created() {
        console.log('composant créé');
    },
    beforeMount() {
        console.log('composant prêt à être ajouté au DOM');
    },
    beforeUpdate() {
        console.log('mise à jour du DOM prête');
    },
    updated() {
        console.log('MAJ DOM effectuée');
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
label {
    display: block;
}

</style>
