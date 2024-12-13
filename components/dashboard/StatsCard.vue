<!-- components/dashboard/StatsCard.vue -->
<template>
  <div class="bg-white mb-6 rounded-lg shadow p-6">
    <div class="flex items-center">
      <div class="p-3 rounded-full" :class="bgColorClass">
        <component :is="icon" class="w-6 h-6 text-white" />
      </div>
      <div class="ml-4">
        <h3 class="text-gray-500 text-sm">{{ title }}</h3>
        <p class="text-2xl font-semibold">{{ value }}</p>
        <p v-if="change" class="text-sm" :class="changeColorClass">
          {{ changePrefix }}{{ change }}%
        </p>
      </div>
    </div>
  </div>
</template>

<script setup lang="ts">
const props = defineProps<{
  title: string;
  value: string | number;
  // eslint-disable-next-line @typescript-eslint/no-explicit-any
  icon: any;
  change?: number;
  color?: 'blue' | 'green' | 'yellow' | 'red' | string;
}>();

const bgColorClass = computed(() => ({
  'bg-blue-500': props.color === 'blue',
  'bg-green-500': props.color === 'green',
  'bg-yellow-500': props.color === 'yellow',
  'bg-red-500': props.color === 'red',
  'bg-blue-100': !props.color,
}));

const changeColorClass = computed(() => ({
  'text-green-500': props.change && props.change > 0,
  'text-red-500': props.change && props.change < 0,
}));

const changePrefix = computed(() =>
  props.change && props.change > 0 ? '+' : ''
);
</script>
