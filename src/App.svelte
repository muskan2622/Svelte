<script>
  import { onMount } from 'svelte';
  
  let latitude = 51.5074; // Default latitude (London)
  let longitude = -0.1278; // Default longitude (London)
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

  function searchWeather() {
    // Convert latitude and longitude inputs to numbers
    const lat = parseFloat(latitude);
    const lon = parseFloat(longitude);

    // Check if latitude and longitude are valid numbers
    if (!isNaN(lat) && !isNaN(lon)) {
      fetchWeatherData(lat, lon);
    } else {
      console.error('Invalid latitude or longitude');
    }
  }

  async function fetchWeatherData(lat, lon) {
    try {
      weatherData = await fetchData(lat, lon, apiKey);
      console.log('Weather data fetched:', weatherData);
    } catch (error) {
      console.error('Error fetching weather data:', error);
    }
  }

  onMount(async () => {
    console.log('Fetching default weather data...');
    fetchWeatherData(latitude, longitude);
  });
</script>

<main>
  <nav class="navbar">
    <h1>Smollan India Weather App</h1>
  </nav>
   <div class="hero-section">
    <div class="hero-content">
      <h2>Check the Weather</h2>
      <div class="search-bar">
        <input type="text" placeholder="Latitude" bind:value={latitude} />
        <input type="text" placeholder="Longitude" bind:value={longitude} />
        <button class="search-button" on:click={searchWeather}>Search</button>
      </div>
    </div>
  </div>
  <div class="weather-container">
    <h2 class="weather-tgit remote set-url originitle">Current Weather</h2>
    {#if weatherData}
      <div class="weather-info">
        <p class="weather-info-item">Temperature: {weatherData.main.temp}K ({(weatherData.main.temp - 273.15).toFixed(2)}°C)</p>
        <p class="weather-info-item">Description: {weatherData.weather[0].description}</p>
        <p class="weather-info-item">Wind Speed: {weatherData.wind.speed} m/s, Direction: {weatherData.wind.deg}°</p>
        <p class="weather-info-item">Humidity: {weatherData.main.humidity}%</p>
        <!-- UV Index is not provided in the response -->
      </div>
    {:else}
      <p>Error fetching weather data.</p>
    {/if}
  </div>
</main>


<style>
  /* Add your CSS styles here */
  main {
    display: flex;
    flex-direction: column;
    align-items: center;
    height: 100vh;
    background-color: #f0f0f0;
  }

  .navbar {
    width: 100%;
    padding: 10px 0;
    background-color: #3b586c;
    color: #fff;
    text-align: center;
  }

  .hero-section {
    width: 100%;
    height: 500px;
    background-image: url('https://m-cdn.phonearena.com/images/article/152061-wide-two_1200/Google-adds-weather-integration-to-the-Clock-app-on-some-Pixel-models.jpg');
    background-size: cover;
    display: flex;
    justify-content: center;
    align-items: center;
    color: #fff;
  }

  .hero-content {
    text-align: center;
  }

  .hero-content h2 {
    font-size: 36px;
    margin-bottom: 20px;
  }

  .search-bar {
    display: flex;
    justify-content: center;
    align-items: center;
    margin-top: 20px;
  }

  .search-bar input {
    padding: 10px;
    margin: 0 10px;
    font-size: 16px;
    border-radius: 5px;
    border: 1px solid #ccc;
    width: 150px;
  }

  .search-button {
    padding: 10px 20px;
    font-size: 16px;
    background-color: #333;
    color: #fff;
    border: none;
    border-radius: 5px;
    cursor: pointer;
  }

  .weather-container {
    background-color: #fff;
    padding: 20px;
    border-radius: 10px;
    box-shadow: 0px 0px 10px rgba(0, 0, 0, 0.1);
    margin-top: 20px;
  }

  .weather-title {
    font-size: 24px;
    font-weight: bold;
    margin-bottom: 10px;
    color: #333;
  }

  .weather-info {
    display: flex;
    flex-direction: column;
  }

  .weather-info-item {
    margin: 5px 0;
    font-size: 16px;
    color: #666;
  }
</style>

