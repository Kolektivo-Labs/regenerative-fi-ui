<script setup lang="ts">
import { flatten } from 'lodash';
import { computed } from 'vue';

import usePoolSwapsQuery from '@/composables/queries/usePoolSwapsQuery';
import { Pool } from '@/services/pool/types';

import Table from './Table.vue';

/**
 * TYPES
 */
type Props = {
  pool: Pool;
  loading: boolean;
};

/**
 * PROPS
 */
const props = withDefaults(defineProps<Props>(), {
  loading: false,
});

/**
 * QUERIES
 */
const poolSwapsQuery = usePoolSwapsQuery(props.pool.id);

/**
 * COMPUTED
 */
const poolSwaps = computed(() =>
  poolSwapsQuery.data.value
    ? flatten(poolSwapsQuery.data.value.pages.map(page => page.poolSwaps))
    : []
);
const isLoadingPoolSwaps = computed(() => poolSwapsQuery.isLoading.value);
const poolSwapsHasNextPage = computed(() => poolSwapsQuery.hasNextPage?.value);
const poolSwapsIsFetchingNextPage = computed(
  () => poolSwapsQuery.isFetchingNextPage?.value
);

/**
 * METHODS
 */
function loadMorePoolSwaps() {
  poolSwapsQuery.fetchNextPage();
}
</script>

<template>
  <Table
    :poolSwaps="poolSwaps"
    :isLoading="loading || isLoadingPoolSwaps"
    :isLoadingMore="poolSwapsIsFetchingNextPage"
    :isPaginated="poolSwapsHasNextPage"
    :noResultsLabel="$t('poolTransactions.noResults.swaps')"
    @load-more="loadMorePoolSwaps"
  />
</template>
