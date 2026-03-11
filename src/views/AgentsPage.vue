<script setup lang="ts">
import { Icon } from '@iconify/vue'
import { Button } from '@/components/ui/button'
import {
  Card,
  CardContent,
  CardDescription,
  CardHeader,
  CardTitle,
} from '@/components/ui/card'
import { Separator } from '@/components/ui/separator'

const benefits = [
  {
    icon: 'ph:copy-duotone',
    title: 'You Own the Code',
    description:
      'Components are copied directly into your project — not installed as a black-box dependency. Customise every pixel, every variant, every behaviour.',
  },
  {
    icon: 'ph:shield-check-duotone',
    title: 'Accessible by Default',
    description:
      'Built on Reka UI (Radix Vue), every component ships with full WAI-ARIA compliance, keyboard navigation, and screen-reader support out of the box.',
  },
  {
    icon: 'ph:code-duotone',
    title: 'TypeScript-First',
    description:
      'Every component, prop, emit, and slot is fully typed. Autocompletion works everywhere — no casting, no any, no surprises at runtime.',
  },
  {
    icon: 'ph:paint-brush-duotone',
    title: 'Fully Customisable',
    description:
      'CSS variables power every colour token. Switch base colour palettes, tweak border radii and shadows globally, or override per-component — all in one file.',
  },
  {
    icon: 'ph:squares-four-duotone',
    title: 'Rich Component Library',
    description:
      '50+ production-ready components covering forms, navigation, overlays, data display, feedback, and layout — all visually consistent and composable.',
  },
  {
    icon: 'ph:terminal-window-duotone',
    title: 'CLI-Powered DX',
    description:
      'Add any component in one command: `npx shadcn-vue@latest add button`. The CLI resolves dependencies, respects your aliases, and drops files exactly where you need them.',
  },
]

const stats = [
  { value: '50+', label: 'Components' },
  { value: 'MIT', label: 'Licensed' },
  { value: 'Reka UI', label: 'Foundation' },
  { value: 'Tailwind v4', label: 'Compatible' },
]
</script>

<template>
  <div class="overflow-y-auto h-full">
    <div class="max-w-4xl mx-auto px-6 py-12 space-y-12">

      <!-- Hero -->
      <section class="space-y-4">
        <div class="flex flex-wrap gap-2 items-center">
          <span class="inline-flex items-center gap-1.5 rounded-full border px-3 py-1 text-xs font-medium text-muted-foreground">
            <Icon icon="ph:star-fill" class="size-3 text-yellow-500" />
            Open Source
          </span>
          <span class="inline-flex items-center gap-1.5 rounded-full border px-3 py-1 text-xs font-medium text-muted-foreground">
            <Icon icon="ph:vuejs-logo-fill" class="size-3 text-green-500" />
            Vue 3
          </span>
          <span class="inline-flex items-center gap-1.5 rounded-full border px-3 py-1 text-xs font-medium text-muted-foreground">
            <Icon icon="ph:check-circle-fill" class="size-3 text-blue-500" />
            Experimental Registry
          </span>
        </div>

        <h1 class="text-4xl font-bold tracking-tight">
          Why shadcn-vue?
        </h1>
        <p class="text-lg text-muted-foreground max-w-2xl">
          Not a component library. Not a dependency. A collection of re-usable components
          that you copy into your codebase and make entirely your own — built on Tailwind CSS
          and Reka UI.
        </p>

        <div class="flex gap-3 pt-2">
          <Button
            as="a"
            href="https://www.shadcn-vue.com"
            target="_blank"
            rel="noopener noreferrer"
          >
            <Icon icon="ph:arrow-square-out" class="size-4 mr-2" />
            Visit shadcn-vue
          </Button>
          <Button
            variant="outline"
            as="a"
            href="https://github.com/radix-vue/shadcn-vue"
            target="_blank"
            rel="noopener noreferrer"
          >
            <Icon icon="ph:github-logo" class="size-4 mr-2" />
            GitHub
          </Button>
        </div>
      </section>

      <Separator />

      <!-- Benefits Grid -->
      <section class="space-y-4">
        <h2 class="text-xl font-semibold">Core Benefits</h2>
        <div class="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-3 gap-4">
          <Card
            v-for="benefit in benefits"
            :key="benefit.title"
            class="transition-shadow hover:shadow-md"
          >
            <CardHeader class="pb-2">
              <div class="flex items-center gap-3">
                <div class="rounded-md bg-primary/10 p-2">
                  <Icon :icon="benefit.icon" class="size-5 text-primary" />
                </div>
                <CardTitle class="text-base">{{ benefit.title }}</CardTitle>
              </div>
            </CardHeader>
            <CardContent>
              <CardDescription class="text-sm leading-relaxed">
                {{ benefit.description }}
              </CardDescription>
            </CardContent>
          </Card>
        </div>
      </section>

      <Separator />

      <!-- Philosophy Callout -->
      <section>
        <Card class="bg-primary text-primary-foreground border-0">
          <CardHeader>
            <div class="flex items-center gap-2">
              <Icon icon="ph:quotes-duotone" class="size-6 opacity-70" />
              <CardTitle class="text-lg">The Copy-Paste Philosophy</CardTitle>
            </div>
          </CardHeader>
          <CardContent class="space-y-3">
            <p class="text-primary-foreground/90 leading-relaxed">
              Traditional component libraries give you a package you import. When it doesn't
              meet your needs you either fork it, fight it, or live with it.
            </p>
            <p class="text-primary-foreground/90 leading-relaxed">
              shadcn-vue flips the model: the CLI copies source code directly into
              <code class="rounded bg-primary-foreground/20 px-1.5 py-0.5 text-sm font-mono">src/components/ui/</code>.
              You read it, tweak it, delete what you don't need. There's nothing to upgrade
              or version-pin — <strong>it's your code now.</strong>
            </p>
            <div class="rounded-lg bg-primary-foreground/10 px-4 py-3 font-mono text-sm mt-4">
              npx shadcn-vue@latest add button
            </div>
          </CardContent>
        </Card>
      </section>

      <!-- Stats Strip -->
      <section>
        <div class="grid grid-cols-2 sm:grid-cols-4 gap-4">
          <div
            v-for="stat in stats"
            :key="stat.label"
            class="rounded-lg border bg-card p-4 text-center space-y-1"
          >
            <p class="text-2xl font-bold tracking-tight">{{ stat.value }}</p>
            <p class="text-xs text-muted-foreground uppercase tracking-wider">{{ stat.label }}</p>
          </div>
        </div>
      </section>

      <!-- Registry CTA -->
      <section>
        <Card class="border-dashed">
          <CardHeader>
            <div class="flex items-center gap-2">
              <Icon icon="ph:plug-duotone" class="size-5 text-primary" />
              <CardTitle class="text-base">This Project's Registry</CardTitle>
            </div>
            <CardDescription>
              The <code class="rounded bg-muted px-1.5 py-0.5 text-xs font-mono">ChatComponent</code> built for this app is published to a local registry and installable in any shadcn-vue project.
            </CardDescription>
          </CardHeader>
          <CardContent>
            <div class="rounded-lg bg-muted px-4 py-3 font-mono text-sm">
              npx shadcn-vue@latest add http://your-domain/r/chat-component.json
            </div>
          </CardContent>
        </Card>
      </section>

    </div>
  </div>
</template>
