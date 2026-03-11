<script setup lang="ts">
import ChatComponent from '../../registry/new-york/chat-component/ChatComponent.vue'
</script>

<template>
  <div class="flex flex-col h-full w-full min-h-0 overflow-hidden">
    <ChatComponent
      ai-initials="SC"
      placeholder="Ask about your analytics..."
      :initial-messages="[
        { id: 1, role: 'ai', content: 'Hi! I\'m your analytics assistant. Ask me anything about your data.' },
      ]"
    />
  </div>
</template>
