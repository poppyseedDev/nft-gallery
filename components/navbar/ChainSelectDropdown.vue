<template>
  <b-dropdown v-model="selected" aria-role="list" :triggers="['click']">
    <template #trigger>
      <div class="navbar-item" data-cy="chain">{{ chainName }}</div>
    </template>
    <b-dropdown-item
      v-for="option in availableChains"
      :key="option.value"
      aria-role="listitem"
      :value="option.value"
      :class="{ 'is-active': selected === option.value }"
      :data-cy="`chain-dropdown-${option.value}`">
      {{ option.text }}
    </b-dropdown-item>
  </b-dropdown>
</template>

<script lang="ts" setup>
import { getChainNameByPrefix } from '@/utils/chain'
import { usePreferencesStore } from '@/stores/preferences'

const { availableChains } = useChain()
const { $store } = useNuxtApp()
const { urlPrefix, setUrlPrefix } = usePrefix()
const prefrencesStore = usePreferencesStore()
const router = useRouter()

const selected = computed({
  get: () => urlPrefix.value,
  set: (value) => {
    setUrlPrefix(value)
    $store.dispatch('setUrlPrefix', value)
    router.push({ path: `/${value}` })
    prefrencesStore.setNotificationBoxCollapse(false)
  },
})

const chainName = computed(() => getChainNameByPrefix(selected.value))
</script>
