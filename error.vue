<script lang="ts" setup>
import type { NuxtError } from '#app'

// prevent reactive update when clearing error
const { error } = defineProps<{
  error: Partial<NuxtError>
}>()

// add more custom status codes messages here
const errorCodes: Record<number, string> = {
  404: 'Page not found',
}

if (process.dev)
  console.error(error)
const defaultMessage = 'Something went wrong'
const message = error.message ?? errorCodes[error.statusCode!] ?? defaultMessage
const state = ref<'error' | 'reloading'>('error')
const reload = async () => {
  state.value = 'reloading'
  try {
    // clearError({ redirect: currentUser.value ? '/home' : `/${currentServer.value}/public/local` })
  }
  catch (err) {
    console.error(err)
    state.value = 'error'
  }
}
</script>

<template>
  <NuxtLayout>
    <slot>
      <span timeline-title-style>Error</span>
      <form p5 grid gap-y-4 @submit="reload">
        <div text-lg>
          Something went wrong
        </div>
        <div text-secondary>
          {{ message }}
        </div>
        <button flex items-center gap-2 justify-center btn-solid text-center :disabled="state === 'reloading'">
          <span v-if="state === 'reloading'" block animate-spin preserve-3d>
            <span block i-ri:loader-2-fill />
          </span>
          {{ state === 'reloading' ? 'Reloading' : 'Reload' }}
        </button>
      </form>
    </slot>
  </NuxtLayout>
</template>

<style lang="scss">

</style>
