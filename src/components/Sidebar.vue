<template>
    <aside :class="[$style.sidebar, { [$style.open]: isOpen }]">
      <nav :class="$style.menu">
        <router-link to="/" :class="$style.menuItem">Dashboard</router-link>
        <router-link to="/assets" :class="$style.menuItem">Assets</router-link>
      </nav>
    </aside>
  </template>
  
  <script>
  export default {
    props: {
      isOpen: {
        type: Boolean,
        default: false,
      },
    },
  }
  </script>
  
  <style module>
  .sidebar {
    width: 200px;
    background-color: #333;
    color: white;
    height: 100%;
    transform: translateX(-100%);
    transition: transform 0.3s ease-in-out;
  }
  
  .open {
    transform: translateX(0);
  }
  
  .menu {
    display: flex;
    flex-direction: column;
    padding: 1rem;
  }
  
  .menuItem {
    color: white;
    text-decoration: none;
    padding: 0.5rem 0;
  }
  
  @media (max-width: 768px) {
    .sidebar {
      position: fixed;
      z-index: 1000;
    }
  }
  </style>