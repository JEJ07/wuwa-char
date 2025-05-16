<script setup>
import { onMounted, ref } from "vue";

const characters = ref([]);
const loading = ref(true);
const error = ref(null);
const editIndex = ref(null);

// V MODEL FIELDS
const addChar = ref("");
const addRole = ref("Support");
const addOwned = ref(false);
const addWeapon = ref("Sword");
const addAttribute = ref("Aero");


const addCharacter = () => {
    if (addChar.value.trim() !== "") {
        characters.value.push({
            name: addChar.value,
            weapon: addWeapon.value,
            attribute: addAttribute.value,
            owned: addOwned.value,
            icon: `https://api.resonance.rest/characters/${encodeURIComponent(addChar.value)}/icon`
        });
        resetForm();
    }
};


const editCharacter = (index) => {
    const char = characters.value[index];
    addChar.value = char.name;
    addWeapon.value = char.weapon;
    addAttribute.value = char.attribute;
    addOwned.value = char.owned;
    editIndex.value = index;
};

const updateCharacter = () => {
    if (editIndex.value !== null) {
        characters.value[editIndex.value] = {
            name: addChar.value,
            weapon: addWeapon.value,
            attribute: addAttribute.value,
            owned: addOwned.value,
            icon: `https://api.resonance.rest/characters/${encodeURIComponent(addChar.value)}/icon`
        };
        resetForm();
    }
};

const resetForm = () => {
    addChar.value = "";
    addWeapon.value = "Sword";
    addAttribute.value = "Aero";
    addOwned.value = false;
    editIndex.value = null;
};

const deleteCharacter = (index) => {
    characters.value.splice(index, 1);
};

onMounted(async () => {
    try {
        const listRes = await fetch("https://api.resonance.rest/characters");
        const listData = await listRes.json();
        const names = listData.characters;

        const fetchDetails = async (name) => {
            const encodedName = encodeURIComponent(name);
            const detailRes = await fetch(`https://api.resonance.rest/characters/${encodedName}`);
            const detailData = await detailRes.json();

            return {
                name: detailData.name,
                weapon: detailData.weapon || "Unknown",
                attribute: detailData.attribute || "Unknown",
                icon: `https://api.resonance.rest/characters/${encodedName}/icon`,
                owned: false
            };
        };

        const promises = names.map(name => fetchDetails(name));
        characters.value = await Promise.all(promises);
    } catch (err) {
        error.value = "Failed to fetch character data.";
        console.error(err);
    } finally {
        loading.value = false;
    }
});
</script>

<template>
    <section class="min-h-screen  bg-slate-900 text-cyan-100 mb-4 p-6 ">
        <div class="container px-6 mx-auto ">
            <h1 class="text-3xl font-bold text-center mb-8 text-cyan-400 border-b border-cyan-800 pb-2">WUWA CHARACTER
                ARCHIVE</h1>

            <!-- Add/Edit Form -->
            <div class="flex justify-center items-center p-4 mb-8">
                <form @submit.prevent="editIndex !== null ? updateCharacter() : addCharacter()"
                    class="w-full max-w-md bg-slate-800 shadow-xl rounded-lg p-6 space-y-4 border border-cyan-700">
                    <h2 class="text-xl font-bold text-center text-cyan-300 mb-4">
                        {{ editIndex !== null ? "EDIT RECORD" : "ADD CHARACTER" }}
                    </h2>

                    <div>
                        <label for="addChar" class="block text-sm font-medium text-cyan-200 mb-1">NAME</label>
                        <input type="text" id="addChar" v-model="addChar" placeholder="Enter name"
                            class="mt-1 block w-full rounded-md bg-slate-700 border border-cyan-900 text-cyan-100 shadow-sm focus:ring-cyan-500 focus:border-cyan-500 p-2" />
                    </div>

                    <div class="grid grid-cols-2 gap-4">
                        <div>
                            <label for="addWeapon"
                                class="block text-sm font-medium text-cyan-200 mb-1">WEAPON</label>
                            <select id="addWeapon" v-model="addWeapon"
                                class="mt-1 block w-full rounded-md bg-slate-700 border border-cyan-900 text-cyan-100 shadow-sm focus:ring-cyan-500 focus:border-cyan-500 p-2">
                                <option>Sword</option>
                                <option>Broadblade</option>
                                <option>Pistols</option>
                                <option>Gauntlet</option>
                                <option>Rectifier</option>
                            </select>
                        </div>

                        <div>
                            <label for="addAttribute"
                                class="block text-sm font-medium text-cyan-200 mb-1">ATTRIBUTE</label>
                            <select id="addAttribute" v-model="addAttribute"
                                class="mt-1 block w-full rounded-md bg-slate-700 border border-cyan-900 text-cyan-100 shadow-sm focus:ring-cyan-500 focus:border-cyan-500 p-2">
                                <option>Aero</option>
                                <option>Spectro</option>
                                <option>Glacio</option>
                                <option>Electro</option>
                                <option>Fusion</option>
                            </select>
                        </div>
                    </div>

                    <div class="flex items-center gap-2">
                        <input id="owned" type="checkbox" v-model="addOwned"
                            class="rounded bg-slate-700 border-cyan-900 text-blue-500 focus:ring-cyan-500" />
                        <label for="owned" class="text-sm text-cyan-200">OWNED</label>
                    </div>

                    <button type="submit"
                        class="w-full bg-blue-800 hover:bg-blue-700 text-cyan-100 font-bold py-2 px-4 rounded-md transition duration-300 border border-blue-900">
                        {{ editIndex !== null ? "UPDATE RECORD" : "ADD CHARACTER" }}
                    </button>
                </form>
            </div>

            <!-- Character Grid -->
            <div v-if="loading" class="text-center text-cyan-300 py-12">
                <div class="inline-block animate-pulse">
                    <div class="text-4xl mb-2">⚡</div>
                    <div>SCANNING DATABASE...</div>
                </div>
            </div>

            <div v-else-if="error" class="text-center text-red-400 py-12">
                <div class="inline-block">
                    <div class="text-4xl mb-2">⚠️</div>
                    <div>DATABASE CONNECTION FAILED</div>
                    <div class="text-sm mt-2 text-red-300">{{ error }}</div>
                </div>
            </div>

            <div v-else class="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-3 xl:grid-cols-4 gap-4 mb-4 p-6">
                <div class="bg-slate-800 rounded-lg shadow-lg p-4 border border-blue-900 hover:shadow-cyan-900/30 transition-all hover:-translate-y-1"
                    v-for="(character, index) in characters" :key="character.name">
                    <div class="flex justify-between items-start mb-3">
                        <h3 class="text-lg font-bold text-cyan-300">
                            {{ character.name.toUpperCase() }}
                        </h3>
                        <span class="text-xs px-2 py-1 rounded-full"
                            :class="character.owned ? 'bg-blue-900/50 text-cyan-300' : 'bg-slate-700 text-slate-400'">
                            {{ character.owned ? 'Owned' : 'Not Owned' }}
                        </span>
                    </div>

                    <img :src="character.icon" :alt="character.name"
                        class="w-24 h-24 object-contain mx-auto mb-3 bg-slate-700/50 rounded-full p-2 border border-cyan-900/50"
                        @error="(e) => e.target.src = 'https://via.placeholder.com/80'" />

                    <div class="space-y-1 text-sm">
                        <p class="flex justify-between border-b border-slate-700/50 pb-1">
                            <span class="text-cyan-200">Weapon:</span>
                            <span class="text-cyan-100">{{ character.weapon }}</span>
                        </p>
                        <p class="flex justify-between border-b border-slate-700/50 pb-1">
                            <span class="text-cyan-200">Attribute:</span>
                            <span class="text-cyan-100">{{ character.attribute }}</span>
                        </p>
                    </div>

                    <div class="mt-4 flex justify-end gap-2">
                        <button @click="editCharacter(index)" title="Edit record"
                            class="text-cyan-400 hover:text-cyan-300 bg-blue-900/30 hover:bg-blue-900/50 p-2 rounded-md transition">
                            <svg xmlns="http://www.w3.org/2000/svg" class="h-5 w-5" fill="none" viewBox="0 0 24 24"
                                stroke="currentColor">
                                <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2"
                                    d="M11 5H6a2 2 0 00-2 2v11a2 2 0 002 2h11a2 2 0 002-2v-5m-1.414-9.414a2 2 0 112.828 2.828L11.828 15H9v-2.828l8.586-8.586z" />
                            </svg>
                        </button>
                        <button @click="deleteCharacter(index)" title="Delete record"
                            class="text-red-400 hover:text-red-300 bg-red-900/30 hover:bg-red-900/50 p-2 rounded-md transition">
                            <svg xmlns="http://www.w3.org/2000/svg" class="h-5 w-5" fill="none" viewBox="0 0 24 24"
                                stroke="currentColor">
                                <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2"
                                    d="M19 7l-.867 12.142A2 2 0 0116.138 21H7.862a2 2 0 01-1.995-1.858L5 7m5 4v6m4-6v6m1-10V4a1 1 0 00-1-1h-4a1 1 0 00-1 1v3M4 7h16" />
                            </svg>
                        </button>
                    </div>
                </div>
            </div>
        </div>
    </section>
</template>