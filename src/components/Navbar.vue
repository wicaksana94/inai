<template>
    <nav :class="$style.navbar">
      <button :class="$style.toggleButton" @click="$emit('toggle-sidebar')">
        Toggle Sidebar
      </button>
      <h1 :class="$style.title">Dashboard</h1>
    </nav>
  </template>
  
  <script>
  export default {
    emits: ['toggle-sidebar'],
  }
  </script>
  
  <style module>
  .navbar {
    display: flex;
    align-items: center;
    padding: 1rem;
    background-color: #f0f0f0;
  }
  
  .toggleButton {
    margin-right: 1rem;
  }
  
  .title {
    margin: 0;
  }
  
  @media (max-width: 768px) {
    .navbar {
      flex-direction: column;
    }
  
    .toggleButton {
      margin-bottom: 0.5rem;
    }
  }
  </style>