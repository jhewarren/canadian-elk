<script setup lang="ts">
import type { mastodon } from 'masto'

defineProps<{
  account: mastodon.v1.Account
  isHoverCard?: boolean
}>()

const userSettings = useUserSettings()
</script>

<template>
  <div flex gap-5>
    <NuxtLink
      :to="getAccountRoute(account)"
      replace
      text-secondary
      exact-active-class="text-primary"
    >
      <template #default="{ isExactActive }">
        <CommonLocalizedNumber
          keypath="account.posts_count"
          :count="account.statusesCount"
          font-bold
          :class="isExactActive ? 'text-primary' : 'text-base'"
        />
      </template>
    </NuxtLink>
    <NuxtLink
      v-if="!(isHoverCard && getPreferences(userSettings, 'hideFollowerCount'))"
      :to="getAccountFollowingRoute(account)"
      replace
      text-secondary exact-active-class="text-primary"
    >
      <template #default="{ isExactActive }">
        <CommonLocalizedNumber
          v-if="!getPreferences(userSettings, 'hideFollowerCount')"
          keypath="account.following_count"
          :count="account.followingCount"
          font-bold
          :class="isExactActive ? 'text-primary' : 'text-base'"
        />
        <span v-else>{{ $t('account.following') }}</span>
      </template>
    </NuxtLink>
    <NuxtLink
      v-if="!(isHoverCard && getPreferences(userSettings, 'hideFollowerCount'))"
      :to="getAccountFollowersRoute(account)"
      replace text-secondary
      exact-active-class="text-primary"
    >
      <template #default="{ isExactActive }">
        <CommonLocalizedNumber
          v-if="!getPreferences(userSettings, 'hideFollowerCount')"
          keypath="account.followers_count"
          :count="account.followersCount"
          font-bold
          :class="isExactActive ? 'text-primary' : 'text-base'"
        />
        <span v-else>{{ $t('account.followers') }}</span>
      </template>
    </NuxtLink>
  </div>
</template>
