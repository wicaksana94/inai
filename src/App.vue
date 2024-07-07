<template>
  <div class="app">
    <Navbar @toggle-sidebar="toggleSidebar" />
    <div class="content">
      <Sidebar :is-open="sidebarOpen" />
      <router-view></router-view>
    </div>
  </div>
</template>

<script>
import { ref } from 'vue';
import Navbar from './components/Navbar.vue';
import Sidebar from './components/Sidebar.vue';

export default {
  components: {
    Navbar,
    Sidebar,
  },
  setup() {
    const sidebarOpen = ref(false);

    const toggleSidebar = () => {
      sidebarOpen.value = !sidebarOpen.value;
    };

    return {
      sidebarOpen,
      toggleSidebar,
    };
  },
}
</script>

<style module>
.app {
  display: flex;
  flex-direction: column;
  height: 100vh;
}

.content {
  display: flex;
  flex: 1;
}
</style>