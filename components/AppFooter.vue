<template>
  <footer class="bg-gray-1000 text-white py-8">
    <div class="max-w-7xl mx-auto grid grid-cols-1 lg:grid-cols-3 gap-8">
      <!-- social media links -->
      <div class="flex flex-col items-start space-y-2">
        <div class="text-lg font-semibold">LET'S CONNECT</div>
        <div class="flex space-x-4">
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
      </div>

      <!-- sponsors. REPLACE LOGOS WHEN SPONSORED -->
      <div class="text-center">
        <div class="text-lg font-semibold">SPONSORED BY</div>
        <div class="flex items-center justify-center space-x-6 mt-2">
          <img
            v-for="(sponsor, index) in sponsors"
            :key="index"
            :src="sponsor.logoPath"
            :alt="sponsor.alt"
            class="h-20"
          >
        </div>
      </div>

      <!-- contact info -->
      <div class="text-right flex flex-col space-y-2">
        <div class="font-semibold text-lg">
          {{ contactInfo.organization }}
        </div>
        <div>{{ contactInfo.address.street }}</div>
        <div>{{ contactInfo.address.cityStateZip }}</div>
        <div>
          <a :href="`mailto:${contactInfo.email}`" class="underline">
            {{ contactInfo.email }}
          </a>
        </div>
      </div>
    </div>
  </footer>
</template>

<script setup lang="ts">
import { FontAwesomeIcon } from '@fortawesome/vue-fontawesome';
import sponsorPlaceholder from '@/assets/img/sponsor_logos/your_logo_here.jpg';

const socialLinks = [
  {
    platform: 'twitter',
    url: 'https://twitter.com/CoogEsports',
    icon: ['fab', 'x-twitter'],
  },
  {
    platform: 'instagram',
    url: 'https://www.instagram.com/CoogEsports',
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

const sponsors = [
  {
    logoPath: sponsorPlaceholder,
    alt: 'Logo0',
  },
  {
    logoPath: sponsorPlaceholder,
    alt: 'Logo1',
  },
  {
    logoPath: sponsorPlaceholder,
    alt: 'Logo2',
  },
];

const contactInfo = {
  organization: 'University of Houston Esports Club',
  address: {
    street: '4465 University Dr. MAILBOX 50',
    cityStateZip: 'Houston, TX 77204',
  },
  email: 'coogesports@gmail.com',
};
</script>
