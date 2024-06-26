<script lang="ts" setup>
import IconGithub from '@/components/icons/IconGithub.vue';
import IconLinkedin from '@/components/icons/IconLinkedin.vue';
import IconMail from '@/components/icons/IconMail.vue';
import IconTelegram from '@/components/icons/IconTelegram.vue';
import IconTwitter from '@/components/icons/IconTwitter.vue';
import { EXTERNAL_LINKS } from '@/constants/links';

import useNetwork from '@/composables/useNetwork';

import { useAppzi } from '@/composables/useAppzi';
import AppLogo from '../images/AppLogo.vue';

const { networkSlug } = useNetwork();
const { openNpsModal } = useAppzi();
</script>

<template>
  <footer v-once>
    <div
      class="xl:container lg:px-4 xl:mx-auto dark:border-t dark:border-gray-800"
    >
      <div
        class="flex flex-col md:flex-row gap-8 md:justify-between py-12 px-4 lg:px-0 text-primary"
      >
        <div class="flex flex-col gap-8">
          <router-link
            class="font-medium link"
            :to="{ name: 'home', params: { networkSlug } }"
          >
            <AppLogo class="mb-4" current="Footer" />
          </router-link>

          <div class="flex md:hidden flex-col gap-2">
            <p>
              <router-link
                class="text-lg font-medium link"
                :to="{ name: 'home', params: { networkSlug } }"
              >
                {{ $t('pool') }}
              </router-link>
            </p>
            <p>
              <router-link
                class="text-lg font-medium link"
                :to="{ name: 'swap', params: { networkSlug } }"
              >
                {{ $t('swap') }}
              </router-link>
            </p>
            <p>
              <router-link
                class="text-lg font-medium link"
                :to="{ name: 'claim', params: { networkSlug } }"
              >
                {{ $t('claim') }}
              </router-link>
            </p>
            <p>
              <router-link
                class="text-lg font-medium link"
                :to="{ name: 'portfolio', params: { networkSlug } }"
              >
                {{ $t('portfolio') }}
              </router-link>
            </p>
            <p>
              <router-link
                class="text-lg font-medium link"
                :to="{ name: 'vebal', params: { networkSlug } }"
              >
                {{ $t('vebal') }}
              </router-link>
            </p>
          </div>

          <div class="flex flex-wrap md:order-3 gap-3 md:gap-4">
            <BalLink
              :href="EXTERNAL_LINKS.RegenerativeFI.Docs"
              external
              noStyle
              class="group link link--external"
            >
              {{ $t('docs') }}
            </BalLink>

            <BalLink
              :href="EXTERNAL_LINKS.RegenerativeFI.Forum"
              external
              noStyle
              class="group link link--external"
            >
              {{ $t('tokens') }}
            </BalLink>

            <BalLink
              :href="EXTERNAL_LINKS.RegenerativeFI.Forum"
              external
              noStyle
              class="group link link--external"
            >
              {{ $t('forum') }}
            </BalLink>

            <BalLink
              :href="EXTERNAL_LINKS.RegenerativeFI.Support"
              external
              noStyle
              class="group link link--external"
            >
              {{ $t('support') }}
            </BalLink>

            <BalLink
              :href="EXTERNAL_LINKS.RegenerativeFI.Analytics"
              external
              noStyle
              class="group link link--external"
            >
              {{ $t('analytics') }}
            </BalLink>
          </div>
        </div>
        <div class="flex flex-col gap-8">
          <div class="md:order-2">
            <div class="flex lg:flex-col md:justify-end">
              <div class="flex gap-3 md:justify-end">
                <BalLink
                  :href="EXTERNAL_LINKS.RegenerativeFI.Social.Twitter"
                  external
                  noStyle
                >
                  <IconTwitter />
                </BalLink>
                <BalLink
                  :href="EXTERNAL_LINKS.RegenerativeFI.Social.Telegram"
                  external
                  noStyle
                >
                  <IconTelegram />
                </BalLink>
                <BalLink
                  :href="EXTERNAL_LINKS.RegenerativeFI.Social.Github"
                  external
                  noStyle
                >
                  <IconGithub />
                </BalLink>
                <BalLink
                  :href="EXTERNAL_LINKS.RegenerativeFI.Social.Linkedin"
                  external
                  noStyle
                >
                  <IconLinkedin />
                </BalLink>
                <BalLink
                  :href="EXTERNAL_LINKS.RegenerativeFI.Social.Mail"
                  external
                  noStyle
                >
                  <IconMail />
                </BalLink>
              </div>
            </div>
          </div>

          <div
            class="flex flex-wrap md:order-3 gap-x-3 lg:gap-x-4 gap-y-2 lg:gap-y-0 md:justify-start h-full"
          >
            <button class="policy" @click="openNpsModal">Feedback</button>
            <router-link class="policy" :to="{ name: 'terms-of-use' }">
              {{ $t('policies.termsOfUse') }}
            </router-link>
            <router-link class="policy" :to="{ name: 'privacy-policy' }">
              {{ $t('policies.privacyPolicy') }}
            </router-link>
            <router-link class="policy" :to="{ name: 'cookies-policy' }">
              {{ $t('policies.cookiesPolicy') }}
            </router-link>
          </div>
        </div>
      </div>
    </div>
  </footer>
</template>

<style scoped>
footer {
  @apply bg-primary-refi dark:bg-gray-900;
}

footer :deep(.logotype) {
  @apply origin-top-left;

  transform: scale(1.5);
}

.link {
  @apply text-white dark:text-white transition-colors flex items-center no-underline hover:text-accent-refi;
}

.link--external {
  @apply text-sm;
}

.link:hover,
.link:focus-visible {
  @apply text-accent-refi dark:text-yellow-500 no-underline;
}

.link:focus:not(:focus-visible) {
  outline: none;
}

.policy {
  @apply text-sm text-white dark:text-gray-400 hover:text-accent-refi dark:hover:text-yellow-500 flex items-end;
}

.arrow {
  @apply text-gray-300 dark:text-gray-600 group-hover:text-accent-refi dark:group-hover:text-yellow-500;
}

.router-link-active {
  @apply text-blue-600 dark:text-blue-400;
}
</style>