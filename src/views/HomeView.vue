<script setup>
import Header from '../components/Header.vue';
import Sidebar from '../components/Sidebar.vue';
import Content from '../components/Content.vue';
</script>

<template>
  <main class="container">
    <Header />
    <div class="container-inner">
      <Sidebar />
      <Content />
    </div>
  </main>
</template>
