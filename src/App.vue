<template>
  <section id="app">
    <app-header/>
    <router-view />
  </section>
</template>

<script>
import AppHeader from '@/components/AppHeader';

export default {
  name: 'app',
  components: { AppHeader }

}
</script>

<style lang="scss">
body {
  margin: 0;
  
}
</style>
