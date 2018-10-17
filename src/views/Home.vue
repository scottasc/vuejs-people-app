<template>

  <div class="home">
    
     <h1>People: {{people.length}}</h1>

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



</style>

<script>
var axios = require('axios');
// @ is an alias to /src
import HelloWorld from '@/components/HelloWorld.vue'

export default {
  data: function() {
    return {
      people: [],
      newPerson: {name: "", bio: "", bioVisible: true}
    };
  },
  created: function() {
    axios
    .get("http://localhost:3000/api/people")
    .then(function(response) {
      this.people = response.data; // this points to an undefined place because it's in an anonymous function and nested away from a direct reference to the Vue object.
    }
    .bind(this));
  },
  methods: {
    addPerson: function() {
      if (this.newPerson.name && this.newPerson.bio) {
        this.people.push(this.newPerson);
        this.newPerson = {name: "", bio: "", completed: false};
    }
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
