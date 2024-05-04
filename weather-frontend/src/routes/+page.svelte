<!-- pages.svelte -->

<script>
    let city = '';
    let weatherData = {};

    async function getWeather() {
        const response = await fetch(`http://localhost:5000/api/weather?city=${city}`);
        weatherData = await response.json();
        console.log(weatherData);
    }
</script>

<div class="container">
    <input type="text" bind:value={city} placeholder="Enter city">
    <button on:click={getWeather}>Get Weather</button>

    {#if Object.keys(weatherData).length > 0}
        <div class="weather-info">
            <h2>Weather Information</h2>
            <p>City: {weatherData.name}</p>
            <p>Temperature: {weatherData.main.temp} C</p>
            <p>Feels Like: {weatherData.main.feels_like} C</p>
            <p>Min Temp: {weatherData.main.temp_min} C</p>
            <p>Max Temp: {weatherData.main.temp_max} C</p>
            <p>Description: {weatherData.weather[0].description}</p>
            <p>Wind Speed: {weatherData.wind.speed} Km/hr</p>
            <p>Humidity: {weatherData.main.humidity} %</p>
            <p>Air Pressure: {weatherData.main.pressure} P</p>
        </div>
    {/if}
</div>


<style>
    /* Custom CSS styles */
    .container {
        max-width: 600px;
        margin: 0 auto;
        padding: 20px;
        border: 1px solid #ccc;
        border-radius: 8px;
        box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
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

    button:hover {
        background-color: #0056b3;
    }

    .weather-info {
        margin-top: 20px;
    }

    .weather-info p {
        margin-bottom: 10px;
    }
</style>
