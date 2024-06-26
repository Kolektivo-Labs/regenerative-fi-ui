<script lang="ts" setup>
import { ref, watch } from 'vue';
import { useI18n } from 'vue-i18n';
import { useRouter } from 'vue-router';

import GithubIcon from '@/components/_global/icons/brands/GithubIcon.vue';
import LinkedinIcon from '@/components/_global/icons/brands/LinkedinIcon.vue';
import TelegramIcon from '@/components/_global/icons/brands/TelegramIcon.vue';
import TwitterIcon from '@/components/_global/icons/brands/TwitterIcon.vue';
import AppLogo from '@/components/images/_AppLogo.vue';
import { version } from '@/composables/useApp';
import { useAppzi } from '@/composables/useAppzi';
import useConfig from '@/composables/useConfig';
import { Goals, trackGoal } from '@/composables/useFathom';
import useNetwork from '@/composables/useNetwork';
import { sleep } from '@/lib/utils';
import useWeb3 from '@/services/web3/useWeb3';
import useDarkMode from '@/composables/useDarkMode';
import { EXTERNAL_LINKS } from '@/constants/links';
/**
 * PROPS & EMITS
 */
const emit = defineEmits(['close']);

/**
 * COMPOSABLES
 */
const { blockNumber } = useWeb3();
const { networkConfig } = useConfig();
const { networkSlug } = useNetwork();
const { t } = useI18n();
const router = useRouter();
const { openNpsModal } = useAppzi();
const { darkMode, toggleDarkMode } = useDarkMode();

/**
 * STATE
 */
const blockIcon = ref<HTMLDivElement>();

const navLinks = [
  { label: t('swap'), path: `/${networkSlug}/swap`, goal: Goals.ClickNavSwap },
  {
    label: t('pool'),
    path: `/pools`,
    goal: Goals.ClickNavPools,
  },
  {
    label: t('dashboard'),
    path: `/${networkSlug}/dashboard`,
    goal: Goals.ClickNavPools,
  },
];

const ecosystemLinks = [
  { label: t('docs'), url: EXTERNAL_LINKS.RegenerativeFI.Docs },
  { label: t('governance'), url: EXTERNAL_LINKS.RegenerativeFI.Vote },
  { label: t('forum'), url: EXTERNAL_LINKS.RegenerativeFI.Forum },
  { label: t('support'), url: EXTERNAL_LINKS.RegenerativeFI.Support },
];

const socialLinks = {
  TwitterIcon: {
    component: TwitterIcon,
    url: 'https://twitter.com/BalancerLabs',
  },
  DiscordIcon: {
    component: TelegramIcon,
    url: 'https://discord.balancer.fi/',
  },
  GithubIcon: {
    url: 'https://github.com/balancer/',
    component: GithubIcon,
  },
  LinkedinIcon: {
    url: 'https://github.com/balancer/',
    component: LinkedinIcon,
  },
};

/**
 * METHODS
 */
function getSocialComponent(componentName) {
  return socialLinks[componentName].component;
}

async function navTo(path: string, goal: string) {
  trackGoal(goal);
  router.push(path);
  emit('close');
}

/**
 * WATCHERS
 */
watch(blockNumber, async () => {
  blockIcon.value?.classList.add('block-change');
  await sleep(300);
  blockIcon.value?.classList.remove('block-change');
});
</script>

<template>
  <div class="opacity-0 fade-in-delay">
    <div
      class="flex flex-col justify-center px-4 h-20 border-b border-gray-800"
    >
      <AppLogo forceDark color="#ffffff" />
    </div>

    <div class="grid mt-2 text-lg grid-col-1">
      <div
        v-for="link in navLinks"
        :key="link.label"
        class="side-bar-link"
        @click="navTo(link.path, link.goal)"
      >
        {{ link.label }}
      </div>
    </div>

    <div class="grid mt-5 text-sm grid-col-1">
      <span class="px-4 pb-1 font-medium text-secondary">Ecosystem</span>
      <BalLink
        v-for="link in ecosystemLinks"
        :key="link.url"
        :href="link.url"
        class="flex items-center side-bar-link"
        external
        noStyle
      >
        {{ link.label }}
        <BalIcon name="arrow-up-right" size="sm" class="ml-1 text-secondary" />
      </BalLink>
      <span class="px-4 pt-1 capitalize" @click="openNpsModal">{{
        t('feedback')
      }}</span>
    </div>

    <div class="px-4 mt-6">
      <div class="mt-2 side-bar-btn" @click="toggleDarkMode">
        <MoonIcon v-if="!darkMode" class="mr-2" />
        <SunIcon v-else class="mr-2" />
        <span>{{ darkMode ? 'Light' : 'Dark' }} mode</span>
      </div>
    </div>

    <div class="grid grid-rows-1 grid-flow-col auto-cols-min gap-2 px-4 mt-4">
      <BalLink
        v-for="(link, componentName) in socialLinks"
        :key="componentName"
        :href="link.url"
        class="social-link"
        noStyle
        external
      >
        <component :is="getSocialComponent(componentName)" />
      </BalLink>
      <BalLink
        href="mailto:contact@balancer.finance"
        class="social-link"
        noStyle
      >
        <EmailIcon />
      </BalLink>
    </div>

    <div class="px-4 mt-6 text-xs">
      <div class="flex items-center">
        <div
          ref="blockIcon"
          class="w-2 h-2 bg-green-500 rounded-full block-icon"
        />
        <span class="ml-2 text-gray-300">
          {{ networkConfig.name }}: Block {{ blockNumber }}
        </span>
      </div>
      <BalLink
        :href="`https://github.com/balancer/frontend-v2/releases/tag/${version}`"
        class="flex items-center mt-2 text-gray-300"
        external
        noStyle
      >
        App: v{{ version }}
        <BalIcon name="arrow-up-right" size="xs" class="ml-1" />
      </BalLink>
    </div>
  </div>
</template>

<style scoped>
.side-bar-link {
  @apply transition duration-300 p-4 py-1.5 hover:bg-gray-850 cursor-pointer;
}

.side-bar-btn {
  @apply flex items-center bg-gray-850 hover:bg-gray-800 rounded-lg p-2 cursor-pointer transition;
}

.social-link {
  @apply w-11 h-11 xs:w-12 xs:h-12  rounded-full bg-gray-850 hover:bg-gray-800 flex items-center justify-center
    text-white cursor-pointer;
}

.social-link > svg {
  @apply w-6 h-6;

  fill: white;
}

.block-icon {
  box-shadow: 0 0 3px 2px theme('colors.green.500');
  transition: box-shadow 0.3s ease-in-out;
}

.block-change {
  box-shadow: 0 0 6px 4px theme('colors.green.500');
}
</style>