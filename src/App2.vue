<!-- COMPOSITION API -->

<script setup>
import { onMounted, ref } from "vue";

const username = ref("Kaoruko");
const status = ref("active");
const role = ref("user");

// const characters = ref([
//   { name: "Shorekeeper", weapon: "Rectifier", attribute: "Spectro", owned: true},
//   { name: "Jinshi", weapon: "Broadblade", attribute: "Spectro", owned: true },
//   { name: "Zani", weapon: "Gauntlet", attribute: "Spectro", owned: false },
//   { name: "Carte", weapon: "Sword", attribute: "Aero", owned: false },
// ]);



const toggleRole = () => {
  role.value =
    role.value === "Admin" ? "user" : role.value === "user" ? "Admin" : "N/A";
};


const posts = ref([]);

onMounted(async () => {
  try {
    const response = await fetch("https://jsonplaceholder.typicode.com/posts");
    const data = await response.json();
    posts.value = data;
  } catch (error) {
    console.log("Error fetching api for posts");
  }
})
</script>

<template>
  <div class="container">
    <header>
      <h1>
        Welcome, {{ username }} <span class="role">({{ role }})</span>
      </h1>
      <p>
        Status: <strong>{{ status }}</strong>
      </p>

      <p v-if="role === 'Admin'">Manage the characters</p>
      <p v-else-if="role === 'user'">See all wuwa characters</p>
      <p v-else>Guest mode</p>
      <button class="role-toggle" @click="toggleRole">Change Role</button>
    </header>
    <div>
      <h1>Posts</h1>
      <ul>
        <li v-for="(post, index) in posts" :key="index">
          {{ post.title }}
        </li>
      </ul>
    </div>

    <div v-if="role === 'Admin'">
      <form
        @submit.prevent="editIndex !== null ? updateCharacter() : addCharacter"
        class="form-section"
      >
        <h2>Add New Character</h2>
        <div class="form-group">
          <label for="addChar">Name</label>
          <input
            type="text"
            id="addChar"
            v-model="addChar"
            placeholder="Enter name"
          />
        </div>

        <div class="form-group">
          <label for="addWeapon">Weapon</label>
          <select id="addWeapon" v-model="addWeapon">
            <option>Sword</option>
            <option>Broadblade</option>
            <option>Pistols</option>
            <option>Gauntlet</option>
            <option>Rectifier</option>
          </select>
        </div>

        <div class="form-group">
          <label for="addAttribute">Attribute</label>
          <select id="addAttribute" v-model="addAttribute">
            <option>Aero</option>
            <option>Spectro</option>
            <option>Glacio</option>
            <option>Electro</option>
            <option>Fusion</option>
          </select>
        </div>

        <div class="form-group checkbox">
          <label>
            <input type="checkbox" v-model="addOwned" />
            Owned
          </label>
        </div>

        <button type="submit">
          {{ editIndex !== null ? "Update Character" : "Add Character" }}
        </button>
      </form>
    </div>

    <section class="character-list">
      <h2>Wuwa Characters</h2>
      <div v-if="role === 'Admin'">
        <div
          v-for="(character, index) in characters"
          :key="character.name"
          class="character-block"
        >
          <h3>{{ character.name }}</h3>
          <p><strong>Weapon:</strong> {{ character.weapon }}</p>
          <p><strong>Attribute:</strong> {{ character.attribute }}</p>
          <p :class="{ owned: character.owned, notOwned: !character.owned }">
            {{ character.owned ? "Character owned" : "Character not owned" }}
          </p>
          <button @click="editCharacter(index)" title="Edit character">
            ✎
          </button>

          <button @click="deleteCharacter(index)" title="Delete character">
            x
          </button>
        </div>
      </div>
    </section>
  </div>

  
</template>

<style scoped>
.container {
  max-width: 700px;
  margin: 0 auto;
  padding: 2rem;
  font-family: Arial, sans-serif;
  background-color: #121212;
  color: #ffffff;
}

header {
  text-align: center;
  margin-bottom: 2rem;
}

.role {
  font-weight: normal;
  font-style: italic;
  color: #bbbbbb;
}

.form-section {
  background-color: #1e1e1e;
  padding: 1.5rem;
  border-radius: 10px;
  margin-bottom: 2rem;
  box-shadow: 0 0 10px rgba(255, 255, 255, 0.05);
}

.form-section h2 {
  margin-bottom: 1rem;
  color: #ffffff;
}

.form-group {
  margin-bottom: 1rem;
  display: flex;
  flex-direction: column;
}

.form-group.checkbox {
  flex-direction: row;
  align-items: center;
  gap: 0.5rem;
}

input[type="text"],
select {
  padding: 0.5rem;
  border-radius: 6px;
  border: 1px solid #444;
  background-color: #2a2a2a;
  color: #fff;
}

input[type="text"]::placeholder {
  color: #aaaaaa;
}

button {
  padding: 0.6rem 1.2rem;
  border: none;
  background-color: #007bff;
  color: white;
  border-radius: 6px;
  cursor: pointer;
  transition: background-color 0.3s ease;
}

button:hover {
  background-color: #0056b3;
}

.character-list {
  margin-top: 2rem;
}

.character-block {
  border: 1px solid #333;
  border-radius: 8px;
  padding: 1rem;
  margin-bottom: 1rem;
  background-color: #1a1a1a;
}

.character-block h3 {
  margin-top: 0;
  color: #ffffff;
}

.owned {
  color: #4caf50; /* green */
}

.notOwned {
  color: #f44336; /* red */
}

.role-toggle {
  display: block;
  margin: 2rem auto 0;
  background-color: #28a745;
}

.role-toggle:hover {
  background-color: #1e7e34;
}
</style>
