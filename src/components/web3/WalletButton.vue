<script setup lang="ts">
import { buildConnectorIconURL } from '@/lib/utils/urls';
import { Wallet, WalletNameMap } from '@/providers/wallet.provider';
import useWeb3 from '@/services/web3/useWeb3';

const props = defineProps<{ wallet: Wallet }>();

const { connectWallet, toggleWalletSelectModal } = useWeb3();
function handleClick() {
  connectWallet(props.wallet);
  toggleWalletSelectModal(false);
}
</script>

<template>
  <button class="wallet-connect-btn" @click="handleClick">
    <div class="flex items-center" style="width: 70%">
      <img :src="buildConnectorIconURL(wallet)" class="mr-4 w-10 h-auto" />
      <h5 class="text-base text-gray-700 dark:text-white">
        <span class="capitalize">{{ WalletNameMap[wallet] }}</span>
      </h5>
    </div>
  </button>
</template>

<style>
.wallet-connect-btn {
  @apply transition-all;
  @apply bg-[#F3FAFF] dark:bg-gray-850 hover:bg-transparent dark:hover:bg-gray-800;
  @apply border hover:border-none hover:shadow-transparent dark:border-gray-900;
  @apply p-4 flex justify-start items-center w-full h-14 rounded-md mb-3 shadow-lg;
}
</style>
