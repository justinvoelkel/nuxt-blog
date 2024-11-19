<script setup lang="ts">
type NavigationItem = {
  title: string;
  to: string;
};

const navigation: NavigationItem[] = [
  {title: 'Home', to: '/'},
  {title: 'Resume', to: '/resume'},
];
</script>

<template>
  <nav>
    <ul>
      <li
        v-for="item in navigation"
        :key="item.to"
      >
        <NuxtLink :to="item.to">{{ item.title }}</NuxtLink>
      </li>
    </ul>
  </nav>
</template>
