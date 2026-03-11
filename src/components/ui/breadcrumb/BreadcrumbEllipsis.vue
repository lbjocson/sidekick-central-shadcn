<script lang="ts" setup>
import type { HTMLAttributes } from "vue"
import { Icon } from "@iconify/vue"
import { cn } from "@/lib/utils"

const props = defineProps<{
  class?: HTMLAttributes["class"]
}>()
</script>

<template>
  <span
    data-slot="breadcrumb-ellipsis"
    role="presentation"
    aria-hidden="true"
    :class="cn('flex size-9 items-center justify-center', props.class)"
  >
    <slot>
      <Icon icon="ph:dots-three" class="size-4" />
    </slot>
    <span class="sr-only">More</span>
  </span>
</template>
