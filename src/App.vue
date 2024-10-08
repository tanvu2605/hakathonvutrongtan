<template>
  <div>
    <Sidebar />
    <router-view></router-view>
  </div>
</template>

<script setup>
import Sidebar from "./components/Sidebar.vue";
</script>

<style>
body,
html {
  margin: 0;
  padding: 60px;
  font-family: Arial, sans-serif;
  background-color: #f0f0f0;
}
</style>
