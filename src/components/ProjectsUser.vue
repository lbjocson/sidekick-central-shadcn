<script setup lang="ts">
import { Icon } from "@iconify/vue"
import { computed } from "vue"

import {
  Select,
  SelectContent,
  SelectItem,
  SelectTrigger,
} from '@/components/ui/select'

const props = defineProps<{
  projects: {
    id: string
    name: string
  }[]
  selectedId?: string
}>()

const emit = defineEmits<{
  "update:selectedId": [selectedId: string]
}>()

const selectedId = computed({
  get: () => props.selectedId ?? props.projects[0]?.id ?? "",
  set: (value) => emit("update:selectedId", value),
})

</script>

<template>
  <Select v-model="selectedId">
    <SelectTrigger
      as-child
      hide-icon
    >
      <Icon
        icon="ph:caret-down"
        class="size-4"
      />
    </SelectTrigger>

    <SelectContent
      side="right"
      class="min-w-56 rounded-lg"
    >
      <SelectItem
        class="cursor-pointer"
        v-for="project in projects"
        :key="project.id"
        :value="project.id"
      >
        {{ project.name }}
      </SelectItem>
    </SelectContent>
  </Select>
</template>
