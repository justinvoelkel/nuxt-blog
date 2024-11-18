<script setup lang="ts">
import type { ParsedContent } from '@nuxt/content';

const perPage = 10;
const page = ref<number>(1);
const posts = ref<ParsedContent[]>([]);

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
            <li v-for="post in posts" :key="post._id">
                <h2>{{ post.title }}</h2>
                <p>{{ post.date }}</p>
                <p>{{ post.excerpt }}</p>
                <nuxt-link :to="post._path">Read more</nuxt-link>
            </li>
        </ul>
        <button @click="fetchMore">Load more</button>
    </main>
</template>