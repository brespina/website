<!-- components/layout/AdminSidebar.vue -->
<template>
  <aside class="fixed pt-2 top-16 left-0 h-screen w-64 bg-lighter-base text-secondary">

    <nav class="mt-4">
      <NuxtLink
        v-for="item in navigationItems"
        :key="item.path"
        :to="item.path"
        class="flex items-center px-6 py-3 text-white hover:bg-red-300 hover:text-white transition-colors"
        :class="{ 'bg-secondary text-white': isCurrentRoute(item.path) }"
      >
        <component :is="item.icon" class="w-5 h-5 mr-3" />
        {{ item.name }}
      </NuxtLink>
    </nav>
  </aside>
</template>

<script setup lang="ts">
import adminLogo0 from '@/assets/img/admin_logo/1.png'
import adminLogo1 from '@/assets/img/admin_logo/admin_view_logo.png'
import {
  HomeIcon,
  UsersIcon,
  DocumentChartBarIcon,
  Cog6ToothIcon,
  PhotoIcon,
  CalendarDaysIcon,
  ComputerDesktopIcon,
} from '@heroicons/vue/24/outline';

const route = useRoute();

const navigationItems = [
  { name: 'Dashboard', path: '/admin', icon: HomeIcon },
  { name: 'Events', path: '/admin/events', icon: CalendarDaysIcon },
  { name: 'Members', path: '/admin/members', icon: UsersIcon },
  { name: 'Matches', path: '/admin/matches', icon: ComputerDesktopIcon },
  { name: 'Media', path: '/admin/media', icon: PhotoIcon },
  { name: 'Finances', path: '/admin/finances', icon: DocumentChartBarIcon },
  { name: 'Settings', path: '/admin/settings', icon: Cog6ToothIcon },
];

const isCurrentRoute = (path: string) => route.path === path;
</script>
