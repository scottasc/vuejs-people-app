<template>

  <div class="home">
    
     <h1>People: {{people.length}}</h1>

     <ul>
       <li v-for="error in errors">{{error}}</li>
     </ul>

      <div v-for="person in people">
          <h2 v-on:click="toggleBio(person)">{{person.name}}</h2>

         <div v-if="person.bioVisible">
          <h3>{{person.bio}}</h3>
          <button v-on:click="deletePerson(person)">delete</button>
         </div>

      </div>

         <div>
          Name: <input v-model="newPerson.name">
          Bio: <input v-model="newPerson.bio">
          <button v-on:click="addPerson()">Add person</button>
        </div>


  </div>

</template>

<style>
  
  .strike {
    text-decoration: line-through;
  }

  body {
    background-color: lightblue;
  }

</style>

<script>
var axios = require('axios');
// @ is an alias to /src
import HelloWorld from '@/components/HelloWorld.vue'

export default {
  data: function() {
    return {
      people: [],
      newPerson: {name: "", bio: "", bioVisible: false},
      errors: []
    };
  },
  created: function() {
    axios
    .get("http://localhost:3000/api/people")
    .then(response => {
      this.people = response.data;
    })
  },
  methods: {
    addPerson: function() {
        this.errors = [];
        var params = {
                      name: this.newPerson.name,
                      bio: this.newPerson.bio
                     };

        axios
        .post("http://localhost:3000/api/people", params)

        .then(response => {
            this.people.push(response.data);
            this.newPerson = {name: "", bio: "", completed: false};
        })

        .catch(error => {
          this.errors = error.response.data.errors;
        });
    },
    toggleBio: function(inputPerson) {
      inputPerson.bioVisible = !inputPerson.bioVisible;
    },
    deletePerson: function (inputPerson) {
      var index = this.people.indexOf(inputPerson);
      this.people.splice(index, 1);
    }
  },
  computed: {}
};
</script>
