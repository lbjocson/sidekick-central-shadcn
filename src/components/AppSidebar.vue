<script setup lang="ts">
import type { SidebarProps } from '@/components/ui/sidebar'
import { Icon } from "@iconify/vue"
import { ref } from "vue"
import NavUser from '@/components/NavUser.vue'
import ProjectsUser from '@/components/ProjectsUser.vue'
import {
  Sidebar,
  SidebarContent,
  SidebarFooter,
  SidebarGroup,
  SidebarGroupContent,
  SidebarHeader,
  SidebarMenu,
  SidebarMenuButton,
  SidebarMenuItem,
} from '@/components/ui/sidebar'
import { RouterLink, useRoute } from 'vue-router'

const props = withDefaults(defineProps<SidebarProps>(), {
  collapsible: "icon",
})

const route = useRoute()

const projects = [
  { id: "sail", name: "Sail Company" },
  { id: "ne_ny", name: "Ne_ny" },
  { id: "sidekick", name: "Sidekick Labs" },
]

const selectedProjectId = ref(projects[0]?.id ?? "")

// This is sample data
const data = {
  user: {
    name: "shadcn",
    email: "m@example.com",
    avatar: "/avatars/shadcn.jpg",
  },
  navMain: [
    {
      title: "Homepage",
      href: "/",
      icon: "ph:chats-circle",
    },
    {
      title: "Agents",
      href: "/agents",
      icon: "ph:robot",
    },
    {
      title: "Analytics",
      href: "/analytics",
      icon: "ph:chart-bar-horizontal",
    },
  ],
}

</script>

<template>
  <Sidebar
    class="overflow-hidden *:data-[sidebar=sidebar]:flex-row"
    v-bind="props"
  >
    <!-- This is the first sidebar -->
    <!-- We disable collapsible and adjust width to icon. -->
    <!-- This will make the sidebar appear as icons. -->
    <Sidebar
      collapsible="none"
      class="w-[calc(var(--sidebar-width-icon)+1px)]! border-r"
    >
      <SidebarHeader>
        <SidebarMenu>
          <SidebarMenuItem>
            <SidebarMenuButton
              size="lg"
              as-child
              class="md:h-8 md:p-0"
            >
              <a href="#">
                <div
                  class="bg-sidebar-primary text-sidebar-primary-foreground flex aspect-square size-8 items-center justify-center rounded-lg"
                >
                  <Icon
                    icon="ph:command"
                    class="size-4"
                  />
                </div>
                <div class="grid flex-1 text-left text-sm leading-tight">
                  <span class="truncate font-medium">Acme Inc</span>
                  <span class="truncate text-xs">Enterprise</span>
                </div>
              </a>
            </SidebarMenuButton>
          </SidebarMenuItem>
        </SidebarMenu>
      </SidebarHeader>
      <SidebarContent>

        <SidebarGroup>
          <SidebarGroupContent class="px-1.5 md:px-0">
            <SidebarMenu>
              <SidebarMenuItem>
                <ProjectsUser
                  :projects="projects"
                  v-model:selectedId="selectedProjectId"
                />
              </SidebarMenuItem>
              <SidebarMenuItem
                v-for="item in data.navMain"
                :key="item.title"
              >
                <SidebarMenuButton
                  as-child
                  :tooltip="item.title"
                  :is-active="route.path === item.href"
                  class="px-2.5 md:px-2"
                >
                  <RouterLink :to="item.href">
                    <Icon
                      :icon="route.path === item.href ? `${item.icon}-fill` : item.icon"
                      class="size-4"
                    />
                    <span>{{ item.title }}</span>
                  </RouterLink>
                </SidebarMenuButton>
              </SidebarMenuItem>
            </SidebarMenu>
          </SidebarGroupContent>
        </SidebarGroup>
      </SidebarContent>
      <SidebarFooter>
        <NavUser :user="data.user" />
      </SidebarFooter>
    </Sidebar>

  </Sidebar>
</template>
