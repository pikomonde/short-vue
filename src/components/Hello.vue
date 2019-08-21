<template>
  <div>
    <b-card
      v-bind:title="titleWithName"
      style="max-width: 20rem;"
      class="mb-2"
    >
      <b-card-text>
        Hey {{ nameAlias }}!
      </b-card-text>
      <input v-model="nameAlias">
    </b-card>
    
    <b-card
      title="Your Number"
      style="max-width: 20rem;"
      class="mb-2"
    >
      <b-card-text>
        <div>
          Color number {{ counter }} is
        </div>
        <div v-if="counter === 1">
          RED
        </div>
        <div v-else-if="counter === 2">
          GREEN
        </div>
        <div v-else>
          BLUE
        </div>
      </b-card-text>
    </b-card>
    
    <b-card
      title="Your Pets"
      style="max-width: 20rem;"
      class="mb-2"
    >
      <input v-model="newPet" v-on:keyup.enter="addPet">
      <b-button v-on:click="addPet">
        ADD
      </b-button>
      <b-card-text>
        <ul>
          <li v-for="pet in pets" v-bind:key="pet">
            {{ pet | capitalize }}
          </li>
        </ul>
      </b-card-text>
    </b-card>

    
    <div>Directives:</div>
    <ul>
      <li>v-model</li>
      <li>v-if</li>
      <li>v-else-if</li>
      <li>v-else</li>
      <li>v-show</li>
      <li>v-text</li>
      <li>v-html</li>
      <li>v-once</li>
      <li>v-for</li>
    </ul>
  </div>
</template>

<script>
export default {
  name: 'hello',
  data: function() {
    return {
      nameAlias: 'Piko',
      counter: 3,
      pets: [
        "Girrafe",
        "Cat",
        "Dog",
        "Turtle",
        "Comodo",
      ],
      newPet: '',
    }
  },
  methods: {
    addPet: function() {
      this.pets.push( this.newPet )
      this.newPet = ""
    }
  },
  filters: {
    capitalize: function(val) {
      return (val[0] || "").toUpperCase() + val.substr(1) .toLowerCase()
    }
  },
  computed: {
    titleWithName: function() {
      if (this.nameAlias.length > 0) {
        return "Hello " + this.nameAlias + "!"
      }
      return "Hello!"
    }
  },
}
</script>