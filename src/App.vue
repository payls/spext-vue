<template>
  <main>
    <div class="spext-container">
      <VideoCont />
    </div>
  </main>
</template>

<script>
import VideoCont from './components/VideoCont.vue'

export default {
  name: 'App',
  components: {
    VideoCont
  }
}
</script>

<style>
  @import './assets/css/main.css';
  @import './assets/css/responsive.css';
</style>