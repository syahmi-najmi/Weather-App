**API Key and URL Declaration**

const apiKey = 'api key'; // OpenWeather Map
const apiUrl = 'https://api.openweathermap.org/data/2.5/weather';

- **apiKey**: This is the API key you received from OpenWeatherMap. It allows you to access their weather data.
- **apiUrl**: This is the base URL for the OpenWeatherMap API's "current weather data" endpoint.

**DOM Element Selection**


const locationInput = document.getElementById('locationInput');
const searchButton = document.getElementById('searchButton');
const locationElement = document.getElementById('location');
const temperatureElement = document.getElementById('temperature');
const descriptionElement = document.getElementById('description');


- **locationInput**: This selects the input field where the user will type the location (city name, for example) they want to get the weather for.
- **searchButton**: This selects the button that the user clicks to initiate the search.
- **locationElement, temperatureElement, descriptionElement**: These select the HTML elements where you want to display the results (location name, temperature, and weather description).

**Adding a Click Event Listener**


searchButton.addEventListener('click', () => {
    const location = locationInput.value;
    if (location) {
        fetchWeather(location);
    }
});


- **searchButton.addEventListener('click')**: This adds an event listener to the "Search" button. When the button is clicked, the function inside this event listener will run.
- **locationInput.value**: This retrieves the text the user entered into the location input field (for example, "New York").
- **if (location)**: This checks if the user actually entered something. If the field is not empty, the `fetchWeather()` function is called, passing in the location entered by the user.

**Fetching Weather Data from the API**


function fetchWeather(location) {
    const url = `${apiUrl}?q=${location}&appid=${apiKey}&units=metric`;

    fetch(url)
    .then(response => response.json())
    .then(data => {
        locationElement.textContent = data.name;
        temperatureElement.textContent = `${Math.round(data.main.temp)}°C`;
        descriptionElement.textContent = data.weather[0].description;
    })
    .catch(error => {
        console.error('Error fetching weather data:', error);
    });
}


- **fetchWeather(location)**: This function is responsible for making the actual API request and updating the page with the results.

- **url**: Inside the function, we construct the full URL that will be used to fetch data from the OpenWeather API. The `${location}` dynamically inserts the location the user entered, and `${apiKey}` inserts your API key. The `&units=metric` ensures that the temperature is returned in Celsius.

- **fetch(url)**: This is the JavaScript function that makes an HTTP request to the API. It sends a request to the `url` we constructed.

- **response.json()**: Once the fetch request is successful, the response (data from the server) is converted from JSON format to a JavaScript object.

- **data.name**: This contains the name of the location (e.g., "New York") and is displayed in the `locationElement` on the page.

- **data.main.temp**: This contains the temperature in Celsius, rounded using `Math.round()`. It is displayed in the `temperatureElement`.

- **data.weather[0].description**: This contains a short description of the weather (e.g., "clear sky"), and it is displayed in the `descriptionElement`.

- **.catch(error)**: If there’s an error during the fetch request (e.g., network problems or invalid location), the error will be logged to the console.

### How It All Works Together:
1. The user types a location (city name) into the input field.
2. The user clicks the "Search" button, which triggers the `click` event listener.
3. The input value (location) is passed to the `fetchWeather()` function.
4. The function constructs a request URL and sends it to the OpenWeatherMap API using `fetch()`.
5. Once the data is fetched, it updates the page with the location name, temperature, and weather description.
6. If anything goes wrong during the API request (invalid city, no internet, etc.), an error is logged.