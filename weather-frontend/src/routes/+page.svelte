<!-- pages.svelte -->

<script>
  let city = "";
  let weatherData = {};
  let citySuggestions = [];

  async function getWeather() {
    const response = await fetch(
      `http://localhost:5000/api/weather?city=${city}`
    );
    weatherData = await response.json();
    console.log(weatherData);
  }

  async function getCitySuggestions(keyword) {
    const response = await fetch(
      `http://localhost:5000/api/cities?keyword=${keyword}`
    );
    citySuggestions = await response.json();
    console.log(citySuggestions);
  }

  function handleInput(event) {
    const keyword = event.target.value.trim();
    if (keyword !== "") {
      getCitySuggestions(keyword);
    } else {
      citySuggestions = [];
    }
  }
</script>

<div class="container">
  <div class="input-container">
    <input type="text" bind:value={city} placeholder="Enter city" on:input={handleInput} />
    <button on:click={getWeather}>Get Weather</button>
  </div>

  {#if Object.keys(weatherData).length > 0}
    <div class="weather-info">
      <div class="temperature-info">
        <p class="temperature">{weatherData.main.temp}°</p>
        <p class="feels-like">Feels Like: {weatherData.main.feels_like}°C</p>
        <p class="city-info">{weatherData.name}</p>
      </div>
      <div class="weather-details">
        <div class="left-details">
          <p>Min Temp: {weatherData.main.temp_min}°C</p>
          <p>Max Temp: {weatherData.main.temp_max}°C</p>
          <p>Air Pressure: {weatherData.main.pressure} P</p>
        </div>
        <div class="right-details">
          <p>Humidity: {weatherData.main.humidity}%</p>
          <p>Wind Speed: {weatherData.wind.speed} Km/hr</p>
        </div>
      </div>
    </div>
  {/if}
</div>

<style>
  /* Add your custom styles here */
  .container {
    max-width: 900px;
    margin: 0 auto;
    padding: 20px;
    border-radius: 8px;
    box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
    background-image: url("https://upload.wikimedia.org/wikipedia/commons/thumb/6/6c/Free_Sky_%285596435888%29.jpg/800px-Free_Sky_%285596435888%29.jpg");
    background-color: #fff;
    display: flex;
    flex-direction: column;
    align-items: center;
  }

  .input-container {
    display: flex;
    align-items: center;
    margin-bottom: 20px;
  }

  input[type="text"] {
    padding: 10px;
    margin-right: 10px;
    border-radius: 4px;
    border: 1px solid #ccc;
    font-size: 16px;
    width: 200px;
  }

  button {
    padding: 10px 20px;
    border: none;
    border-radius: 4px;
    background-color: #007bff;
    color: #fff;
    font-size: 16px;
    cursor: pointer;
  }

  .weather-info {
    text-align: center;
    margin-top: 20px;
    display: flex;
    justify-content: space-between;
    width: 100%;
  }

  .city-info {
    font-size: 1.1rem;
  }

  .temperature-info {
    flex: 1;
    text-align: center;
  }

  .temperature {
    font-size: 3rem;
    margin-bottom: 10px;
  }

  .feels-like {
    font-size: 1rem;
  }

  .weather-details {
    flex: 1;
    display: grid;
    grid-template-rows: repeat(2, 1fr);
    gap: 10px;
    text-align: left;
  }
</style>
