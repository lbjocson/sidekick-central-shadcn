<script setup lang="ts">
import { onMounted, ref, watch } from 'vue'
import AppSidebar from '@/components/AppSidebar.vue'
import { SidebarInset, SidebarProvider, SidebarTrigger } from '@/components/ui/sidebar'
import { Toaster } from '@/components/ui/sonner'
import 'vue-sonner/style.css'
import { RouterView } from 'vue-router'

const theme = ref<'light' | 'dark'>('light')

const setThemeClass = (value: 'light' | 'dark') => {
  const root = document.documentElement
  root.classList.toggle('dark', value === 'dark')
}

const initTheme = () => {
  const stored = localStorage.getItem('theme')
  if (stored === 'light' || stored === 'dark') {
    theme.value = stored
    setThemeClass(stored)
    return
  }

  const prefersDark = globalThis.matchMedia?.('(prefers-color-scheme: dark)').matches
  theme.value = prefersDark ? 'dark' : 'light'
  setThemeClass(theme.value)
}

onMounted(initTheme)

watch(theme, (value) => {
  localStorage.setItem('theme', value)
  setThemeClass(value)
})
</script>

<template>
  <Toaster />
  <SidebarProvider
    :default-open="false"
    :style="{
      '--sidebar-width': '3rem',
    }"
    class="h-screen"
  >
    <AppSidebar />
    <SidebarInset class="flex flex-col min-h-0 h-full">
      <div class="relative flex flex-1 flex-col min-h-0 w-full overflow-hidden">
        <div class="absolute left-4 top-4 z-20 md:hidden">
          <SidebarTrigger />
        </div>
        <RouterView class="flex-1 min-h-0 w-full overflow-hidden" />
      </div>
    </SidebarInset>
  </SidebarProvider>
</template>
