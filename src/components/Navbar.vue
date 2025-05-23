<template>
    <nav :class="$style.navbar">
      <button :class="$style.toggleButton" @click="$emit('toggle-sidebar')">
        Toggle Sidebar
      </button>
      <h1 :class="$style.title">Dashboard</h1>
      <div :class="$style.weatherInfo">
        <div v-if="isLoading">Weather loading...</div>
        <div v-else-if="error">{{ error }}</div>
        <div v-else-if="weather">
          <span>{{ weather.city }}: {{ weather.temp }}°C, {{ weather.description }}</span>
          <img :src="`https://openweathermap.org/img/wn/${weather.icon}.png`" alt="Weather icon" />
        </div>
      </div>
    </nav>
  </template>
  
  <script>
  export default {
    emits: ['toggle-sidebar'],
    data() {
      return {
        weather: null,
        isLoading: true,
        error: null,
      };
    },
    methods: {
      async fetchWeather() {
        this.isLoading = true;
        this.error = null;
        if (!navigator.geolocation) {
          this.error = "Geolocation is not supported by your browser.";
          this.isLoading = false;
          return;
        }

        navigator.geolocation.getCurrentPosition(
          async (position) => {
            const { latitude, longitude } = position.coords;
            const apiKey = 'YOUR_API_KEY'; // Placeholder
            const url = `https://api.openweathermap.org/data/2.5/weather?lat=${latitude}&lon=${longitude}&appid=${apiKey}&units=metric`;

            try {
              const response = await fetch(url);
              if (!response.ok) {
                // Attempt to parse error response from API if available
                let apiErrorMessage = `HTTP error! status: ${response.status}`;
                try {
                    const errorData = await response.json();
                    apiErrorMessage = errorData.message || apiErrorMessage;
                } catch (e) {
                    // Ignore if error response is not JSON
                }
                throw new Error(apiErrorMessage);
              }
              const data = await response.json();
              this.weather = {
                temp: data.main.temp,
                description: data.weather[0].description,
                icon: data.weather[0].icon,
                city: data.name,
              };
            } catch (error) {
              console.error("Error fetching weather:", error);
              this.error = `Could not retrieve weather: ${error.message}`;
            } finally {
              this.isLoading = false;
            }
          },
          (error) => {
            console.error("Error getting location:", error);
            this.error = "Could not retrieve location. Please ensure location access is allowed.";
            this.isLoading = false;
          }
        );
      },
    },
    mounted() {
      this.fetchWeather();
    },
  }
  </script>
  
  <style module>
  .navbar {
    display: flex;
    align-items: center;
    justify-content: space-between; /* This will push title to left and weather to right */
    padding: 1rem;
    background-color: #f0f0f0;
  }
  
  .weatherInfo {
    /* Add any specific styling here if needed, e.g., margin-left */
    margin-left: 1rem; /* Adjust as needed */
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