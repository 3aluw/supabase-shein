<template>
  <div>
    <NuxtPage />
  </div>
</template>
<script setup>
useHead({
  htmlAttrs: {
    lang: 'ar',
    dir: 'rtl'
  },
})
</script>
<style>
html{
  direction: rtl;
  font-family: sans-serif;
}
</style>
