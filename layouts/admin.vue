<!-- layouts/admin.vue -->
<template>
  <div class="min-h-screen bg-[#3c3c3c]">
    <AdminSidebar
      :class="{ '-translate-x-full': !showSidebar }"
      class="transition-transform duration-300 lg:translate-x-0"
    />

    <main
      :class="{ 'lg:ml-64': showSidebar }"
      class="transition-all duration-300"
    >
      <AdminHeader @toggle-sidebar="toggleSidebar" />

      <div class="p-6">
        <slot />
      </div>
    </main>
  </div>
</template>

<script setup lang="ts">
import AdminHeader from '~/components/layout/AdminHeader.vue';
import AdminSidebar from '~/components/layout/AdminSidebar.vue';
const showSidebar = ref(true);

const toggleSidebar = () => {
  showSidebar.value = !showSidebar.value;
};
</script>
