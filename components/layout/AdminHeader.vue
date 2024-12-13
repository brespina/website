<!-- components/layout/AdminHeader.vue -->
<template>
  <header class="absolute inset-x-0 top-0 bg-lighter-base shadow-sm">
    <div class="flex items-center justify-between px-6 py-2">
      <div class="flex items-center">
        <button
          class="lg:hidden p-2 rounded-md hover:bg-gray-100"
          @click="toggleSidebar"
        >
          <Bars3Icon class="w-6 h-6 text-black font-thin" />
        </button>
          <NuxtLink to="/admin">
            <img :src="adminLogo" alt="admin_logo" style="width: 30%; height: auto">
          </NuxtLink>

      </div>

      <div class="flex items-center space-x-4">
        <button class="flex items-center space-x-2 p-2 rounded-md hover:bg-gray-200 ">
          <BellIcon class="w-6 h-6 text-white font-thin" />
        </button>
        <div v-click-outside="closeUserMenu" class="relative">
          <button
            class="flex items-center space-x-2 p-2 rounded-md hover:bg-gray-200 "
            @click="toggleUserMenu"
          >
            <UserIcon class="w-6 h-6 text-white font-thin" />
            <ChevronDownIcon class="w-6 h-6 text-white font-thin" />
          </button>
          <div
            v-show="showUserMenu"
            class="absolute right-0 mt-2 w-48 bg-white rounded-md shadow-lg py-1"
          >
            <a
              v-for="item in userMenuItems"
              :key="item.name"
              href="#"
              class="block px-4 py-2 text-sm text-black hover:bg-gray-200"
              @click="item.action"
            >
              {{ item.name }}
            </a>
          </div>
        </div>
      </div>
    </div>
  </header>
</template>

<script setup lang="ts">
import adminLogo from '@/assets/img/admin_logo/admin_logo-2x1.png'
import {
  Bars3Icon,
  BellIcon,
  UserIcon,
  ChevronDownIcon,
} from '@heroicons/vue/24/outline';

const route = useRoute();
const showUserMenu = ref(false);

const pageTitle = computed(() => {
  const routeName = route.name?.toString() || '';
  return routeName.charAt(0).toUpperCase() + routeName.slice(1);
});

const userMenuItems = [
  {
    name: 'Profile',
    action: () => navigateTo('/admin/profile'),
  },
  {
    name: 'Settings',
    action: () => navigateTo('/admin/settings'),
  },
  {
    name: 'Logout',
    action: () => console.log('Logout'),
  },
];

const toggleUserMenu = () => {
  showUserMenu.value = !showUserMenu.value;
};

const closeUserMenu = () => {
  showUserMenu.value = false;
};

const emit = defineEmits(['toggle-sidebar']);
const toggleSidebar = () => emit('toggle-sidebar');
</script>
