<script setup lang="ts">
import { ref, computed, nextTick } from 'vue'
import { Avatar, AvatarFallback } from '@/components/ui/avatar'
import { Button } from '@/components/ui/button'
import { Icon } from '@iconify/vue'
import {
  InputGroup,
  InputGroupAddon,
  InputGroupButton,
  InputGroupTextarea,
} from '@/components/ui/input-group'

const scrollAreaRef = ref<HTMLElement | null>(null)

// Chat state
const inputMessage = ref('')
const messages = ref([
  {
    id: 1,
    role: 'user',
    content: 'Hello! Can you help me with my project?',
  },
  {
    id: 2,
    role: 'ai',
    content: "Of course! I'd be happy to help. What would you like to work on today?",
  },
  {
    id: 3,
    role: 'user',
    content: 'I need to refactor some Vue components',
  },
  {
    id: 4,
    role: 'ai',
    content: "I can definitely help with that. Could you tell me more about which components you'd like to refactor and what improvements you're looking for?",
  },
])

// Computed property to check if send button should be enabled
const canSend = computed(() => inputMessage.value.trim().length > 0)

// Send message handler
const sendMessage = () => {
  if (!canSend.value) return

  // Add user message
  messages.value.push({
    id: messages.value.length + 1,
    role: 'user',
    content: inputMessage.value.trim(),
  })

  // Clear input
  inputMessage.value = ''

  // Scroll to bottom
  nextTick(() => {
    if (scrollAreaRef.value) {
      scrollAreaRef.value.scrollTop = scrollAreaRef.value.scrollHeight
    }
  })

  // Simulate AI response after a short delay
  setTimeout(() => {
    messages.value.push({
      id: messages.value.length + 1,
      role: 'ai',
      content: "This is a dummy AI response. In a real application, this would connect to an AI service.",
    })

    // Scroll to bottom after AI response
    nextTick(() => {
      if (scrollAreaRef.value) {
        scrollAreaRef.value.scrollTop = scrollAreaRef.value.scrollHeight
      }
    })
  }, 500)
}
</script>

<template>
  <header class="bg-background sticky top-0 flex shrink-0 items-center gap-2 p-2">
    <Button
      class="ml-auto text-xs"
      variant="outline"
      size="sm"
    >
      Clear Conversation
    </Button>
  </header>
  <div class="flex flex-col h-full w-full min-h-0 overflow-hidden">
    <!-- Chat Messages Area -->
    <div
      ref="scrollAreaRef"
      class="flex-1 min-h-0 overflow-y-auto"
    >
      <div class="max-w-3xl mx-auto px-4 py-12 space-y-4">
        <div
          v-for="message in messages"
          :key="message.id"
          :class="[
            'flex gap-3 items-start',
            message.role === 'user' ? 'justify-end' : 'justify-start flex-col',
          ]"
        >
          <!-- AI Avatar (shown on left for AI messages) -->
          <Avatar
            v-if="message.role === 'ai'"
            class="size-6 flex-shrink-0 mt-0.5"
          >
            <AvatarFallback class="bg-primary text-primary-foreground text-xs">
              AI
            </AvatarFallback>
          </Avatar>

          <!-- Message Bubble -->
          <div :class="[
            'max-w-[80%]',
            message.role === 'user'
              ? 'rounded-lg px-3 py-3 bg-muted'
              : 'max-w-[100%]',
          ]">
            <p class="text-sm leading-normal">{{ message.content }}</p>
          </div>

        </div>
      </div>
    </div>

    <!-- Input Group at Bottom -->
    <div class="border-t bg-background p-3">
      <div class="max-w-3xl mx-auto">
        <InputGroup>
          <InputGroupTextarea
            v-model="inputMessage"
            placeholder="Ask, Search or Chat..."
            @keydown.enter.exact.prevent="sendMessage"
            class="min-h-[48px] max-h-[100px] resize-none"
          />
          <InputGroupAddon align="block-end">
            <InputGroupButton
              variant="outline"
              size="icon-xs"
            >
              <Icon
                icon="ph:question"
                class="size-4"
              />
            </InputGroupButton>

            <InputGroupButton
              variant="default"
              class="ml-auto"
              size="icon-sm"
              :disabled="!canSend"
              @click="sendMessage"
            >
              <Icon
                icon="ph:paper-plane-right-fill"
                class="size-4"
              />
              <span class="sr-only">Send</span>
            </InputGroupButton>
          </InputGroupAddon>
        </InputGroup>
      </div>
    </div>
  </div>
</template>
