<script setup>
import { ref, defineProps } from 'vue';
import GhibliCard from './slots/GhibliCard.vue';
const setFirstName = ref('Emman');
const setJob = ref(['Fulltime God']);
const setAspiringJob = ref(['Aspiring Developer']);
const setJobIntro = ref('I craft intuitive and responsive interfaces while bringing backend logic to life.');
const setImage = ref('https://i.pinimg.com/736x/4b/a9/0d/4ba90d88454ce9fb035710adaf55d793.jpg');
const Title = ref('TEST');
defineProps({
    title: {
        type: String,
        default: 'Not Set'
    },
    subtitle: {
        type: String,
        default: 'Not set'
    },
    intro: {
        type: String,
        default: 'Not set'
    },
    image: {
        type: String,
        default: 'placeholder.jpg'
    }
});
</script>


<template>
    <section class="pt-24 pb-16 md:pt-32 md:pb-24 ghibli-section-light relative overflow-hidden">
        <!-- ANIMATED BG ELEM -->
        <div class="absolute inset-0 overflow-hidden">
            <div class="parallax absolute w-64 h-64 bg-ghibliPink/20 rounded-full -top-32 -left-32" data-speed="0.3">
            </div>
            <div class="parallax absolute w-96 h-96 bg-ghibliBlue/20 rounded-full -bottom-48 -right-48"
                data-speed="0.2"></div>
            <div class="parallax absolute w-48 h-48 bg-ghibliGreen/20 rounded-full top-1/2 left-1/2 transform -translate-x-1/2 -translate-y-1/2"
                data-speed="0.4"></div>
        </div>

        <div class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8 relative">
            <div class="md:flex items-center">
                <!-- LEFT -->
                <div class="md:w-1/2 mb-10 md:mb-0 ">
                    <GhibliCard>
                        <h1 class="text-4xl md:text-5xl font-bold text-ghibliBrown mb-4">
                            Hi, I'm <span
                                class="text-ghibliGreen border-b-2 border-ghibliGreen pb-1 inline-block transform hover:scale-110 transition-transform">{{
                                    title }}</span>
                        </h1>
                        <h2 class="text-2xl md:text-3xl font-semibold text-ghibliTextDark mb-3">{{ subtitle }}</h2>
                        <p class="text-ghibliText mb-8 text-lg max-w-lg">{{ intro }}</p>
                        <div class="flex space-x-4">
                            <a href="#portfolio"
                                class="ghibli-button transform hover:scale-105 transition-transform ">View Work</a>
                            <a href="#contact"
                                class="px-6 py-3 rounded-full border-2 border-ghibliGreen text-ghibliGreen hover:bg-ghibliGreen hover:text-white transition transform hover:scale-105">Contact
                                Me</a>
                        </div>
                    </GhibliCard>
                </div>


                <!-- RIGHT -->
                <div class="md:w-1/2 flex justify-center">
                    <div class="relative w-64 h-64 md:w-80 md:h-80">
                        <img :src="image" alt=""
                            class="absolute  transform hover:scale-105 transition-transform duration-500 inset-0 w-full h-full object-cover rounded-full border-4 border-ghibliGold shadow-lg transform hover:scale-105 transition-transform duration-500">
                    </div>
                </div>
            </div>


        </div>


    </section>
</template>