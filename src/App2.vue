<!-- <script>
export default{
  data() {
    return {
      name: 'Shorekeeper',
      role: 'Support',
      owned: true
    };
  },
};
</script> -->

<!-- OPTIONS API -->
<script>
export default{
  data() {
    return {
      link: 'https://www.prydwen.gg/wuthering-waves/characters/',
      user: {
        username: 'Kaoruko',
        role: 'Admin'
      },
      characters: [
        { name: 'Shorekeeper', role: 'Support', owned: true },
        { name: 'Jinshi', role: 'DPS', owned: false },
        { name: 'Zani', role: 'DPS', owned: true },
        { name: 'Carte', role: 'DPS', owned: false }
      ]
    };
  },
  methods: {
    toggleRole(){
      if (this.user.role === 'Admin') {
          this.user.role ='user';
      } else if(this.user.role === 'user') {
        this.user.role ='Admin'
      } else{
        this.user.role = 'N/A'
      }
    }
  },
};
</script>

<template>
  <header>
    <h1>Welcome, {{ user.username }} - ({{ user.role }})</h1>
    <p v-if="user.role ==='Admin'">Manage the characters</p>
    <p v-else-if="user.role ==='user'">See all wuwa characters</p>
    <p v-else>Guest mode</p>
  </header>
  <br>
  <h1>Wuwa Characters</h1>
  <br>
  <div v-for="character in characters" :key="character.name" class="character-block">
    <h2>{{ character.name }}</h2>
    <p>Role: {{ character.role }}</p>
    <p v-if="character.owned">Character owned</p>
    <p v-else>Character not owned</p>
  </div>
  <!-- <a v-bind:href="link" target="_blank">See more detailed characters</a> -->
  <a :href="link" target="_blank">See more detailed characters</a>
  <!-- <button v-on:click="toggleRole">Change role</button> -->
  <button @click="toggleRole">Change role</button>
</template>

<style scoped>
.character-block {
  margin-bottom: 1.5rem; 
}
</style>