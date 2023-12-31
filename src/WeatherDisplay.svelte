<!-- WeatherDisplay.svelte -->
<script>
  import { onMount } from 'svelte';

  const lat = 51.5074; // Example latitude (London)
  const lon = -0.1278; // Example longitude (London)
  const apiKey = 'c685801635d2b619a59a4b856f94f36b'; // Replace with your actual API key

  let weatherData;

  async function fetchData(lat, lon, apiKey) {
    const url = `https://api.openweathermap.org/data/2.5/weather?lat=${lat}&lon=${lon}&appid=${apiKey}`;
  
    try {
      const response = await fetch(url);
      const data = await response.json();
      return data;
    } catch (error) {
      throw new Error('Error fetching weather data: ' + error.message);
    }
  }

  onMount(async () => {
    console.log('Fetching weather data...');
    try {
      weatherData = await fetchData(lat, lon, apiKey);
      console.log('Weather data fetched:', weatherData);
    } catch (error) {
      console.error('Error fetching weather data:', error);
    }
  });
</script>



<div>
  <h2 class="text-2xl font-bold">Current Weather</h2>

  
  {#if weatherData}
    <p>Temperature: {weatherData.temperature}°F ({weatherData.temperatureCelsius}°C)</p>
    <p>Description: {weatherData.description}</p>
    <p>Wind Speed: {weatherData.windSpeed} mph, Direction: {weatherData.windDirection}</p>
    <p>Humidity: {weatherData.humidity}%</p>
    <p>UV Index: {weatherData.uvIndex}</p>
  {:else}
    <p>Error fetching weather data.</p>
  {/if}
</div>
