<!-- pages.svelte -->

<script>
  let city = "";
  let weatherData = {};
  let citySuggestions = [];
  let showSuggestions = false;

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
      showSuggestions = true;
    } else {
      citySuggestions = [];
      showSuggestions = false;
    }
  }

   /*  Function to handle selection of a city suggestion */
   function handleSuggestionClick(cityName) {
    city = cityName;
    showSuggestions = false;
    getWeather();
  }

</script>

<div class="container">
  <div class="input-container">
    <input type="text" bind:value={city} placeholder="Enter city" on:input={handleInput} />
    <button on:click={getWeather}>Get Weather</button>
    <!-- Suggestions dropdown -->
  {#if showSuggestions && citySuggestions.length > 0}
  <ul class="suggestions">
    {#each citySuggestions as suggestion}
      <li on:click={() => handleSuggestionClick(suggestion)}>{suggestion}</li>
    {/each}
  </ul>
{/if}
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
  position: relative; /* Set input-container to relative positioning */
  display: inline-block; /* Change display to inline-block */
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

  .suggestions {
  position: absolute;
  width: auto; /* Set width to auto to fit content */
  max-width: 100%; /* Set max-width to 100% to prevent overflow */
  max-height: 200px;
  overflow-y: auto;
  border: 1px solid #ccc;
  border-top: none;
  background-color: #fff;
  z-index: 1;
  margin-top: 10px; /* Adjust margin top to create space between input and dropdown */
  padding-left: 0;
  list-style-type: none;
  left: 0; /* Align dropdown with left edge of input */
}
  .suggestions li {
    padding: 5px 10px;
    cursor: pointer;
  }

  .suggestions li:hover {
    background-color: #f0f0f0;
  }
</style>
