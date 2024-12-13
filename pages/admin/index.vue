<!-- pages/admin/index.vue -->
<template>
  <div class="absolute">
    <div class="relative top-20 grid grid-cols-1 md:grid-cols-2 lg:grid-cols-4 gap-6 mb-8">
      <StatsCard v-for="stat in stats" :key="stat.title" v-bind="stat" />
    </div>

    <div class="relative flex mt-20 bg-white rounded-lg shadow">
      <div class="p-6 border-b border-gray-200">
        <h3 class="text-lg font-semibold">Recent Activity</h3>
      </div>
      <div class="p-6">
        <div v-for="stat in stats" :key="stat.title" v-bind="stat">
          {{ stat }}
        </div>
      </div>
    </div>
  </div>
</template>

<script setup lang="ts">
import StatsCard from '~/components/dashboard/StatsCard.vue';
import {
  UsersIcon,
  ShoppingCartIcon,
  CurrencyDollarIcon,
  ChartBarIcon,
} from '@heroicons/vue/24/outline';

const stats = [
  {
    title: 'Total Users',
    value: '1,234',
    icon: UsersIcon,
    color: 'blue',
    change: 12.5,
  },
  {
    title: 'Total Orders',
    value: '456',
    icon: ShoppingCartIcon,
    color: 'green',
    change: -2.3,
  },
  {
    title: 'Revenue',
    value: '$12,345',
    icon: CurrencyDollarIcon,
    color: 'yellow',
    change: 8.4,
  },
  {
    title: 'Growth',
    value: '+12.3%',
    icon: ChartBarIcon,
    color: 'red',
    change: 4.2,
  },
];

definePageMeta({
  layout: 'admin',
});
</script>
