<template>
  <div class="flex h-screen">
    <main class="flex-1 bg-gray-1000 p-4">
      <div class="p-4 bg-gray-1000 text-white min-h-screen">
        <div class="bg-gray-1000 p-6 rounded-lg shadow-md mb-6">
          <h2 class="text-xl font-semibold mb-4">Create New Match</h2>
          <form class="space-y-4" @submit.prevent="createMatch">
            <div class="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-4 gap-4">
              <div class="col-span-1">
                <label for="team1" class="block text-gray-300">Team 1</label>
                <input
                  id="team1"
                  v-model="newMatch.team1"
                  type="text"
                  placeholder="Enter Team 1 Name"
                  class="mt-1 block w-full p-2 h-12 border border-gray-1000 bg-gray-1000 rounded text-black"
                >
              </div>
              <div class="col-span-1">
                <label for="team2" class="block text-gray-300">Team 2</label>
                <input
                  id="team2"
                  v-model="newMatch.team2"
                  type="text"
                  placeholder="Enter Team 2 Name"
                  class="mt-1 block w-full p-2 h-12 border border-gray-1000 bg-gray-1000 rounded text-black"
                >
              </div>
              <div class="col-span-1">
                <label for="date" class="block text-gray-300">Date</label>
                <input
                  id="date"
                  v-model="newMatch.date"
                  type="date"
                  class="mt-1 block w-full p-2 h-12 border border-gray-1000 bg-gray-1000 rounded text-black"
                >
              </div>
              <div class="col-span-1">
                <label for="time" class="block text-gray-300">Time</label>
                <input
                  id="time"
                  v-model="newMatch.time"
                  type="time"
                  class="mt-1 block w-full p-2 h-12 border border-gray-1000 bg-gray-1000 rounded text-black"
                >
              </div>
              <div class="col-span-1">
                <label for="matchType" class="block text-gray-300">Match Type</label>
                <input
                  id="matchType"
                  v-model="newMatch.matchType"
                  type="text"
                  placeholder="Enter Match Type"
                  class="mt-1 block w-full p-2 h-12 border border-gray-1000 bg-gray-1000 rounded text-black"
                >
              </div>
            </div>
            <button type="submit" class="bg-red-600 text-white px-4 py-2 rounded hover:bg-red-700 mt-4">Create Match</button>
          </form>
        </div>

        <!-- Upcoming Matches Table -->
        <div class="bg-gray-1000 p-6 rounded-lg shadow-md">
          <div class="mb-4">
            <button class="bg-red-600 text-white px-4 py-2 rounded hover:bg-red-700" @click="deleteSelectedMatches">Delete Selected</button>
          </div>
          <table class="min-w-full bg-gray-1000">
            <thead>
              <tr class="bg-gray-1000 text-gray-1000">
                <th class="py-2 px-4 border-b border-gray-600">
                  <input
                    v-model="selectAll"
                    type="checkbox"
                    class="form-checkbox h-5 w-5 text-gray-1000"
                    @change="toggleSelectAll"
                  >
                </th>
                <th class="py-2 px-4 border-b border-gray-600">Team 1</th>
                <th class="py-2 px-4 border-b border-gray-600">Team 2</th>
                <th class="py-2 px-4 border-b border-gray-600">Date</th>
                <th class="py-2 px-4 border-b border-gray-600">Time</th>
                <th class="py-2 px-4 border-b border-gray-600">Match Type</th>
                <th class="py-2 px-4 border-b border-gray-600">Actions</th>
              </tr>
            </thead>
            <tbody>
              <tr v-for="match in matches" :key="match.id" class="bg-gray-1000 border-b-2 border-gray-600">
                <td class="py-2 px-4 text-center">
                  <input
                    v-model="match.selected"
                    type="checkbox"
                    class="form-checkbox h-5 w-5 text-gray-1000"
                  >
                </td>
                <td class="py-2 px-4 text-center">{{ match.team1 }}</td>
                <td class="py-2 px-4 text-center">{{ match.team2 }}</td>
                <td class="py-2 px-4 text-center">{{ match.date }}</td>
                <td class="py-2 px-4 text-center">{{ match.time }}</td>
                <td class="py-2 px-4 text-center">{{ match.matchType }}</td>
                <td class="py-2 px-4 text-center">
                  <button class="bg-blue-600 text-white px-3 py-1 rounded hover:bg-blue-700" @click="editMatch(match.id)">Edit</button>
                  <button class="bg-red-600 text-white px-3 py-1 rounded hover:bg-red-700 ml-2" @click="confirmDelete(match.id)">Delete</button>
                </td>
              </tr>
            </tbody>
          </table>
        </div>
      </div>
    </main>
  </div>
</template>

<script setup>
import { ref } from 'vue'

const matches = ref([])

const newMatch = ref({
  team1: '',
  team2: '',
  date: '',
  time: '',
  matchType: ''
})

const selectAll = ref(false)

function createMatch() {
  const match = { ...newMatch.value, id: Date.now(), selected: false }
  matches.value.push(match)
  newMatch.value = { team1: '', team2: '', date: '', time: '', matchType: '' }
}

// function editMatch(id) {
//   // Implement edit functionality
// }

function confirmDelete(id) {
  if (window.confirm('Are you sure you want to delete this match?')) {
    deleteMatch(id)
  }
}

function deleteMatch(id) {
  matches.value = matches.value.filter(match => match.id !== id)
}

function deleteSelectedMatches() {
  matches.value = matches.value.filter(match => !match.selected)
}

function toggleSelectAll() {
  matches.value.forEach(match => {
    match.selected = selectAll.value
  })
}

definePageMeta({
  layout: 'admin'
});
</script>


