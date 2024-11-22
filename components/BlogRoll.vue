<script setup lang="ts">
import type {ParsedContent} from '@nuxt/content';
import {useIntersectionObserver} from '@vueuse/core';

const perPage = 8;
const page = ref<number>(1);
const posts = ref<ParsedContent[]>([]);
const trigger = ref<HTMLElement | null>(null);

useIntersectionObserver(trigger, async ([entry]) => {
  if (entry.isIntersecting) {
    await fetchMore();
  }
});

posts.value = await queryContent('posts')
  .sort({date: -1})
  .skip(1)
  .limit(perPage)
  .find();

async function fetchMore() {
  page.value++;
  const nextPage = await queryContent('posts')
    .sort({date: -1})
    .skip(page.value * perPage)
    .limit(perPage)
    .find();

  posts.value = [...posts.value, ...nextPage];
}
</script>

<template>
  <main>
    <h1>Posts</h1>
    <ul>
      <li
        v-for="post in posts"
        :key="post._id"
      >
        <Transition
          name="fade"
          appear
        >
          <section>
            <h2>{{ post.title }}</h2>
            <p>{{ post.date }}</p>
            <p>{{ post.excerpt }}</p>
            <nuxt-link :to="post._path">Read more</nuxt-link>
          </section>
        </Transition>
      </li>
      <span ref="trigger" />
    </ul>
  </main>
</template>

<style scoped>
/* Transition for fade-in effect */
.fade-enter-active,
.fade-leave-active {
  transition:
    opacity 0.6s ease,
    transform 0.6s ease;
}
.fade-enter-from {
  opacity: 0;
  transform: translateY(20px);
}
.fade-enter-to {
  opacity: 1;
  transform: translateY(0);
}
</style>
