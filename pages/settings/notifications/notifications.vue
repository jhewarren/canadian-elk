<script setup lang="ts">
definePageMeta({
  middleware: 'auth',
})

const { t } = useI18n()

useHydratedHead({
  title: () => `${t('settings.notifications.notifications.label')} | ${t('settings.notifications.label')} | ${t('nav.settings')}`,
})
</script>

<template>
  <MainContent back>
    <template #title>
      <div text-lg font-bold flex items-center gap-2 @click="$scrollToTop">
        <span>{{ isHydrated ? $t('settings.notifications.notifications.label') : '' }}</span>
      </div>
    </template>
    <h3 px6 py4 mt2 font-bold text-xl flex="~ gap-1" items-center>
      {{ isHydrated ? $t('settings.notifications.notifications.label') : '' }}
    </h3>
    <p text-4xl text-center>
      <span sr-only>{{ isHydrated ? $t('settings.notifications.under_construction') : '' }}</span> 🚧
    </p>
  </MainContent>
</template>
