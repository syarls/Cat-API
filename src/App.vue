<script setup>
import Cats from "./components/Cats.vue";
</script>

<template>
  <main>
    <Suspense>
      <template #default>
        <Cats />
      </template>
      <template #fallback>
        <div>
          <p>Loading...</p>
        </div>
      </template>
    </Suspense>
  </main>
</template>

<style scoped></style>