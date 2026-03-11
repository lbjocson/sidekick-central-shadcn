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

interface Message {
  id: number
  role: 'user' | 'ai'
  content: string
}

const props = withDefaults(
  defineProps<{
    initialMessages?: Message[]
    placeholder?: string
    aiName?: string
    aiInitials?: string
    /** When true (default), a simulated AI reply is shown if no `send` listener calls addReply. */
    simulate?: boolean
  }>(),
  {
    initialMessages: () => [],
    placeholder: 'Ask, Search or Chat...',
    aiName: 'AI',
    aiInitials: 'AI',
    simulate: true,
  },
)

const emit = defineEmits<{
  send: [message: string, addReply: (reply: string) => void]
}>()

const scrollAreaRef = ref<HTMLElement | null>(null)
const inputMessage = ref('')
const isThinking = ref(false)

const messages = ref<Message[]>(
  props.initialMessages.length > 0
    ? [...props.initialMessages]
    : [
      { id: 1, role: 'user', content: 'Hello! Can you help me with my project?' },
      {
        id: 2,
        role: 'ai',
        content: "Of course! I'd be happy to help. What would you like to work on today?",
      },
    ],
)

const canSend = computed(() => inputMessage.value.trim().length > 0 && !isThinking.value)

const scrollToBottom = () => {
  nextTick(() => {
    if (scrollAreaRef.value) {
      scrollAreaRef.value.scrollTop = scrollAreaRef.value.scrollHeight
    }
  })
}

const addReply = (content: string) => {
  messages.value.push({ id: messages.value.length + 1, role: 'ai', content })
  isThinking.value = false
  scrollToBottom()
}

const sendMessage = () => {
  if (!canSend.value) return

  const text = inputMessage.value.trim()
  messages.value.push({ id: messages.value.length + 1, role: 'user', content: text })
  inputMessage.value = ''
  isThinking.value = true
  scrollToBottom()

  emit('send', text, addReply)

  if (props.simulate) {
    setTimeout(() => {
      addReply(
        "This is a simulated AI response. Wire up the `send` emit to connect a real AI service.",
      )
    }, 600)
  }
}

const clearConversation = () => {
  messages.value = []
}
</script>

<template>
  <div class="flex flex-col h-full w-full min-h-0 overflow-hidden">
    <!-- Header -->
    <header class="bg-background sticky top-0 flex shrink-0 items-center gap-2 p-2 border-b">
      <span class="text-sm font-medium text-muted-foreground ml-1">Chat Component</span>
      <Button
        class="ml-auto text-xs"
        variant="outline"
        size="sm"
        @click="clearConversation"
      >
        Clear Conversation
      </Button>
    </header>

    <!-- Messages Area -->
    <div
      ref="scrollAreaRef"
      class="flex-1 min-h-0 overflow-y-auto"
    >
      <div class="max-w-3xl mx-auto px-4 py-8 space-y-4">
        <div
          v-for="message in messages"
          :key="message.id"
          :class="[
            'flex gap-3 items-start',
            message.role === 'user' ? 'justify-end' : 'justify-start flex-col',
          ]"
        >
          <Avatar
            v-if="message.role === 'ai'"
            class="size-6 flex-shrink-0 mt-0.5"
          >
            <AvatarFallback class="bg-primary text-primary-foreground text-xs">
              {{ aiInitials }}
            </AvatarFallback>
          </Avatar>

          <div :class="[
            'max-w-[80%]',
            message.role === 'user'
              ? 'rounded-lg px-3 py-3 bg-muted'
              : 'max-w-[100%]',
          ]">
            <p class="text-sm leading-normal">{{ message.content }}</p>
          </div>
        </div>

        <!-- Thinking indicator -->
        <div
          v-if="isThinking"
          class="flex gap-3 items-start justify-start"
        >
          <Avatar class="size-6 flex-shrink-0 mt-0.5">
            <AvatarFallback class="bg-primary text-primary-foreground text-xs">
              {{ aiInitials }}
            </AvatarFallback>
          </Avatar>
          <div class="flex gap-1 items-center px-3 py-3">
            <span class="size-1.5 rounded-full bg-muted-foreground/50 animate-bounce [animation-delay:0ms]" />
            <span class="size-1.5 rounded-full bg-muted-foreground/50 animate-bounce [animation-delay:150ms]" />
            <span class="size-1.5 rounded-full bg-muted-foreground/50 animate-bounce [animation-delay:300ms]" />
          </div>
        </div>
      </div>
    </div>

    <!-- Input -->
    <div class="border-t bg-background p-3">
      <div class="max-w-3xl mx-auto">
        <InputGroup>
          <InputGroupTextarea
            v-model="inputMessage"
            :placeholder="placeholder"
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
