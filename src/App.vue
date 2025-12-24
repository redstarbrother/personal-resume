<template>
  <div class="container">
    <div class="content">
      <ProfileCard />
      <Terminals />
    </div>
    <AppFooter />
  </div>
</template>

<script setup>
import ProfileCard from './components/ProfileCard.vue';
import Terminals from './components/Terminals.vue';
import AppFooter from './components/AppFooter.vue';
</script>

<style>
/* Reset & Base - Global Styles */
* {
  box-sizing: border-box;
  margin: 0;
  padding: 0;
}

body {
  font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, "Helvetica Neue", Arial, sans-serif;
  overflow: hidden; /* Hide scrollbars for the main view */
}
</style>

<style scoped>
.container {
  width: 100vw;
  height: 100vh;
  background: url('https://images.unsplash.com/photo-1506905925346-21bda4d32df4?q=80&w=2070&auto=format&fit=crop') no-repeat center center/cover;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  position: relative;
}

.content {
  display: flex;
  align-items: center;
  gap: 50px;
  width: 100%;
  max-width: 1200px;
  padding: 20px;
  justify-content: center;
}

@media (max-width: 900px) {
  .container {
    height: auto;
    min-height: 100vh;
    padding: 40px 0;
  }
  
  .content {
    flex-direction: column;
    gap: 40px;
  }
}
</style>
