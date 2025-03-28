<script setup lang="ts">
import type { mastodon } from 'masto'
import CommonScrollIntoView from '../common/CommonScrollIntoView.vue'

const { items, command } = defineProps<{
  items: mastodon.v1.Account[]
  command: Function
  isPending?: boolean
}>()

let selectedIndex = $ref(0)

watch(items, () => {
  selectedIndex = 0
})

function onKeyDown(event: KeyboardEvent) {
  if (items.length === 0)
    return false

  if (event.key === 'ArrowUp') {
    selectedIndex = ((selectedIndex + items.length) - 1) % items.length
    return true
  }
  else if (event.key === 'ArrowDown') {
    selectedIndex = (selectedIndex + 1) % items.length
    return true
  }
  else if (event.key === 'Enter') {
    selectItem(selectedIndex)
    return true
  }

  return false
}

function selectItem(index: number) {
  const item = items[index]
  if (item)
    command({ id: item.acct })
}

defineExpose({
  onKeyDown,
})
</script>

<template>
  <div v-if="isPending || items.length" relative bg-base text-base shadow border="~ base rounded" text-sm py-2 overflow-x-hidden overflow-y-auto max-h-100>
    <template v-if="isPending">
      <div flex gap-1 items-center p2 animate-pulse>
        <div animate-spin preserve-3d>
          <div i-ri:loader-2-line />
        </div>
        <span>{{ $t('common.fetching') }}</span>
      </div>
    </template>
    <template v-if="items.length">
      <CommonScrollIntoView
        v-for="(item, index) in items" :key="index"
        :active="index === selectedIndex"
        as="button"
        :class="index === selectedIndex ? 'bg-active' : 'text-secondary'"
        block m0 w-full text-left px2 py1
        @click="selectItem(index)"
      >
        <AccountInfo :account="item" />
      </CommonScrollIntoView>
    </template>
  </div>
  <div v-else />
</template>
