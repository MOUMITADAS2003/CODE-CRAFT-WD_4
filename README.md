<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Weather App</title>
    <style>
        body { font-family: Arial, sans-serif; text-align: center; padding: 20px; }
        #weather-info { margin-top: 20px; }
    </style>
</head>
<body>
    <h1>Weather App</h1>
    <input type="text" id="location" placeholder="Enter location">
    <button onclick="fetchWeather()">Get Weather</button>
    <button onclick="getUserLocation()">Use My Location</button>
    <div id="weather-info"></div>

    <script>
        const apiKey = 'YOUR_API_KEY'; // Replace with your API key from OpenWeatherMap
        
        async function fetchWeather(location) {
            if (!location) location = document.getElementById('location').value;
            if (!location) {
                alert('Please enter a location or allow location access.');
                return;
            }
            
            const url = `https://api.openweathermap.org/data/2.5/weather?q=${location}&units=metric&appid=${apiKey}`;
            try {
                const response = await fetch(url);
                const data = await response.json();
                displayWeather(data);
            } catch (error) {
                alert('Error fetching weather data');
            }
        }

        function getUserLocation() {
            if (navigator.geolocation) {
                navigator.geolocation.getCurrentPosition(position => {
                    const { latitude, longitude } = position.coords;
                    fetchWeatherByCoords(latitude, longitude);
                }, () => alert('Geolocation access denied.'));
            } else {
                alert('Geolocation is not supported by your browser.');
            }
        }

        async function fetchWeatherByCoords(lat, lon) {
            const url = `https://api.openweathermap.org/data/2.5/weather?lat=${lat}&lon=${lon}&units=metric&appid=${apiKey}`;
            try {
                const response = await fetch(url);
                const data = await response.json();
                displayWeather(data);
            } catch (error) {
                alert('Error fetching weather data');
            }
        }

        function displayWeather(data) {
            if (data.cod !== 200) {
                document.getElementById('weather-info').innerHTML = `<p>${data.message}</p>`;
                return;
            }
            document.getElementById('weather-info').innerHTML = `
                <h2>${data.name}, ${data.sys.country}</h2>
                <p>${data.weather[0].description}</p>
                <p>Temperature: ${data.main.temp}°C</p>
                <p>Humidity: ${data.main.humidity}%</p>
                <p>Wind Speed: ${data.wind.speed} m/s</p>
            `;
        }
    </script>
</body>
</html>
