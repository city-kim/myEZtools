<script setup lang="ts">
import { RouterLink, useRouter } from 'vue-router'
const router = useRouter()
</script>

<template>
  <header>
    <nav>
      <RouterLink
        v-for="list in router.getRoutes()"
        :to="list.path"
      >{{ list.name }}</RouterLink>
    </nav>
  </header>
</template>