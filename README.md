# assignment
this is my first repository
<br>
aouter sibtain
import React, { useState, useEffect } from 'react';
import './styles/App.css';

const API_KEY = 'YOUR_OPENWEATHERMAP_API_KEY';

const App = () => {
  const [weatherData, setWeatherData] = useState(null);
  const [error, setError] = useState(null);

  useEffect(() => {
    const fetchData = async () => {
      try {
        const response = await fetch(
          `https://api.openweathermap.org/data/2.5/weather?q=${weatherData}&appid=${API_KEY}`
        );
        const data = await response.json();
        setWeatherData(data);
      } catch (error) {
        setError('Error fetching weather data');
      }
    };

    if (weatherData) {
      fetchData();
    }
  }, [weatherData]);

  return (
    <div className="App">
      <h1>Weather App</h1>
       <WeatherForm setWeatherData={setWeatherData} />
    error && <ErrorMessage message={error} />
      {weatherData && <WeatherCard data={weatherData} />}
    </div>
  );
};
const WeatherForm = ({ setWeatherData }) => {
  const cityInputRef = useRef();

  const handleSubmit = (e) => {
    e.preventDefault();
    const city = cityInputRef.current.value;
    setWeatherData(city);
  };

  return (
    <form onSubmit={handleSubmit}>
      <label>
        Enter City:
        <input type="text" ref={cityInputRef} />
      </label>
      <button type="submit">Get Weather</button>
    </form>
  );
};



const WeatherCard = ({ data }) => {
  return (
    <div className="WeatherCard">
      <h2>{data.name}</h2>
      <p>Temperature: {data.main.temp}°C</p>
      <p>Weather: {data.weather[0].description}</p>
    </div>
  );
};
