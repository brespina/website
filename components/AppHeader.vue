<template>
  <div class="max-w-7xl mx-auto grid grid-cols-1 lg:grid-cols-3 gap-8">
    <div class="flex space-x-6 items-center">
      <a
        v-for="social in socialLinks"
        :key="social.platform"
        :href="social.url"
        target="_blank"
        rel="noopener noreferrer"
        class="text-2xl text-secondary"
      >
        <font-awesome-icon :icon="social.icon" />
      </a>
    </div>

    <a href="/" class="flex justify-center">
      <img
        src="/assets/img/coogesports_w_text.png"
        class="w-32 h-auto"
        alt="Coog Esports"
      >
    </a>

    <div class="flex space-x-6 items-center justify-end">
      <a href="/login" class="text-2xl justify-self-end text-secondary">
        <font-awesome-icon :icon="['fas', 'user']" />
      </a>
    </div>
  </div>

  <!-- main navigation links centered below -->
  <div class="mt-4 max-w-7xl mx-auto">
    <hr class="h-px my-4 bg-gray-200 border-0 dark:bg-white" >
    <div class="flex justify-center mb-4 space-x-6">
      <NuxtLink
        v-for="link in navLinks"
        :key="link.to"
        :to="link.to"
        exact-active-class="border border-primary"
        class="text-white uppercase px-4 py-2"
      >
        {{ link.label }}
      </NuxtLink>
    </div>
  </div>
</template>

<script setup lang="ts">
import { FontAwesomeIcon } from '@fortawesome/vue-fontawesome';

const socialLinks = [
  {
    platform: 'twitter',
    url: 'https://x.com/coogesports',
    icon: ['fab', 'x-twitter'],
  },
  {
    platform: 'instagram',
    url: 'https://www.instagram.com/coogesports/',
    icon: ['fab', 'instagram'],
  },
  {
    platform: 'twitch',
    url: 'https://www.twitch.tv/coogesports',
    icon: ['fab', 'twitch'],
  },
  {
    platform: 'discord',
    url: 'https://discord.com/invite/coog-gaming-154441500950921216',
    icon: ['fab', 'discord'],
  },
];

const navLinks = [
  { to: '/', label: 'Home' },
  { to: '/teams', label: 'Teams' },
  { to: '/events', label: 'Events' },
  { to: '/matches', label: 'Matches' },
  { to: '/media', label: 'Media' },
  { to: '/contact', label: 'Contact' },
];
</script>

<style scoped>
.nuxt-link-exact-active {
  border: 1px solid var(--primary-color); /* no idea why i needed this, too scared to change */
}
</style>
