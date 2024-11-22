<script setup lang="ts">
import {navigationMenuTriggerStyle} from './ui/navigation-menu';
import NavigationMenu from './ui/navigation-menu/NavigationMenu.vue';

type NavigationItem = {
  title: string;
  to: string;
};

const navigation: NavigationItem[] = [
  {title: 'Posts', to: '/'},
  {title: 'Resume', to: '/resume'},
];
</script>

<template>
  <nav>
    <NavigationMenu>
      <NavigationMenuList>
        <NavigationMenuItem
          v-for="item in navigation"
          :key="item.to"
        >
          <NuxtLink :to="item.to">
            <NavigationMenuLink :class="navigationMenuTriggerStyle()">
              {{ item.title }}
            </NavigationMenuLink>
          </NuxtLink>
        </NavigationMenuItem>
      </NavigationMenuList>
    </NavigationMenu>
  </nav>
</template>
