<script setup>
import Navbar from './components/Navbar.vue';
import Hero from './components/Hero.vue';
import CharacterList from './components/CharacterList.vue';
import TaskList from './components/TaskList.vue';
</script>

<template>
  <Navbar />
  <Hero title="Kaoruko" subtitle="Aspiring Developer"
  image="https://i.pinimg.com/736x/4b/a9/0d/4ba90d88454ce9fb035710adaf55d793.jpg" intro="I create beautiful trash — messy, spaghetti code."/>

  <CharacterList />
  <TaskList />


</template>
<style scoped></style>