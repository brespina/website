<template>
  <div class="flex h-screen">
    <main class="flex-1 bg-[#050a10] p-4">
      <div class="p-4 bg-gray-1000 text-white min-h-screen">
        <div class="bg-gray-1000 p-6 rounded-lg shadow-md mb-6">
          <h2 class="text-xl font-arimo mb-4">Create New Event</h2>
          <form class="space-y-4" @submit.prevent="addEvent">
            <div class="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-4 gap-4">
              <div>
                <label for="eventId" class="block text-gray-300">Event ID</label>
                <input
                  id="eventId"
                  v-model="newEvent.id"
                  type="text"
                  placeholder="Enter Event ID"
                  class="mt-1 block w-full p-2 h-12 border border-gray-1000 bg-gray-1000 rounded text-black"
                >
              </div>
              <div>
                <label for="eventName" class="block text-gray-300">Event Title</label>
                <input
                  id="eventName"
                  v-model="newEvent.name"
                  type="text"
                  placeholder="Enter Event Title"
                  class="mt-1 block w-full p-2 h-12 border border-gray-1000 bg-gray-1000 rounded text-black"
                >
              </div>
              <div>
                <label for="eventDate" class="block text-gray-300">Date of Event</label>
                <input
                  id="eventDate"
                  v-model="newEvent.date"
                  type="date"
                  class="mt-1 block w-full p-2 h-12 border border-gray-1000 bg-gray-1000 rounded text-black"
                >
              </div>
              <div>
                <label for="eventLocation" class="block text-gray-300">Event Location</label>
                <input
                  id="eventLocation"
                  v-model="newEvent.location"
                  type="text"
                  placeholder="Enter Event Location"
                  class="mt-1 block w-full p-2 h-12 border border-gray-1000 bg-gray-1000 rounded text-black"
                >
              </div>
              <div>
                <label for="eventGoogleForm" class="block text-gray-300">Event Google Form</label>
                <input
                  id="eventGoogleForm"
                  v-model="newEvent.googleForm"
                  type="url"
                  placeholder="Enter Event Google Form" 
                  class="mt-1 block w-full p-2 h-12 border border-gray-1000 bg-gray-1000 rounded text-black"
                >
              </div>

            </div>
            <button type="submit" class="bg-[#2c2c2c] text-white px-4 py-2 rounded hover:bg-secondary hover:text-white transition-colors duration-300 mt-4">Add Event</button>
          </form>
        </div>
    
        <!-- Upcoming Events Table -->
        <div class="bg-gray-1000 p-6 rounded-lg shadow-md">
          <div class="mb-4">
            <button class="bg-[#2c2c2c] text-white px-4 py-2 rounded hover:bg-secondary hover:text-white transition-colors duration-300 mt-4" @click="confirmDeleteSelected">Delete Selected</button>
          </div>
          <table class="min-w-full bg-gray-1000 ">
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
                <th class="py-2 px-4 border-b border-gray-600">Event ID</th>
                <th class="py-2 px-4 border-b border-gray-600">Event Name</th>
                <th class="py-2 px-4 border-b border-gray-600">Date</th>
                <th class="py-2 px-4 border-b border-gray-600">Location</th>
                <th class="py-2 px-4 border-b border-gray-600">Google Form</th>
              </tr>
            </thead>
            <tbody>
              <tr v-for="event in events" :key="event.id" class="bg-gray-1000 border-b-2 border-gray-600">
                <td class="py-2 px-4 text-center">
                  <input
                    v-model="event.selected"
                    type="checkbox"
                    class="form-checkbox h-5 w-5 text-gray-800"
                  >
                </td>
                <td class="py-2 px-4 text-center">{{ event.id }}</td>
                <td class="py-2 px-4 text-center">{{ event.name }}</td>
                <td class="py-2 px-4 text-center">{{ event.date }}</td>
                <td class="py-2 px-4 text-center">{{ event.location }}</td>
                <td class="py-2 px-4 text-center">
                  <a
                    :href="event.googleForm" target="_blank"
                    rel="noopener noreferrer"
                    class="hover:underline"
                  >
                    <button class="bg-[#2c2c2c] rounded-md p-1 hover:bg-secondary hover:text-white transition-colors duration-300">Sign In Form</button>
                  </a>
                </td>
                <td class="py-2 px-4 text-center">
                  <!-- <button class="bg-red-600 text-white px-3 py-1 rounded hover:bg-red-700" @click="confirmDeleteEvent(event.id)">Delete</button> -->
                </td>
              </tr>
            </tbody>
          </table>
        </div>
      </div>
    </main>
  </div>
</template>

<script setup lang="ts">
interface Event {
  id: string;
  name: string;
  date: string;
  location: string;
  googleForm: string;
  selected: boolean;
}

const newEvent = ref<Omit<Event, 'selected'>>({
  id: '',
  name: '',
  date: '',
  location: '',
  googleForm: ''
});

const events = ref<Event[]>([]);
const selectAll = ref(false);

function addEvent(): void {
  if (newEvent.value.name && newEvent.value.date && newEvent.value.location) {
    events.value.push({ ...newEvent.value, selected: false });
    newEvent.value = { id: '', name: '', date: '', location: '', googleForm: '' };
  }
}

// eslint-disable-next-line @typescript-eslint/no-unused-vars
function confirmDeleteEvent(id: string): void {
  if (window.confirm('Are you sure you want to delete this event?')) {
    deleteEvent(id);
  }
}

function deleteEvent(id: string): void {
  events.value = events.value.filter((event) => event.id !== id);
}

function confirmDeleteSelected(): void {
  if (window.confirm('Are you sure you want to delete the selected events?')) {
    deleteSelected();
  }
}

function deleteSelected(): void {
  events.value = events.value.filter((event) => !event.selected);
  selectAll.value = false;
}

function toggleSelectAll(): void {
  events.value.forEach((event) => (event.selected = selectAll.value));
}

// Page meta
definePageMeta({
  layout: 'admin'
});
</script>

<style scoped>
/* Tailwind CSS is used, no additional styles required */
</style>
