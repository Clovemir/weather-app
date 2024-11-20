<template>
  <div id="app" class="weather-app">
    <header>
      <h1>🌤️ Atividade pra ver o Clima</h1>
      <input
        type="text"
        v-model="city"
        placeholder="Digite o nome da cidade..."
        @keyup.enter="fetchWeather"
      />
      <button @click="fetchWeather">Buscar</button>
    </header>

    <main v-if="weatherData">
      <div class="weather-card">
        <h2>{{ weatherData.name }}, {{ weatherData.sys.country }}</h2>
        <p class="temperature">{{ Math.round(weatherData.main.temp) }}°C</p>
        <p>{{ weatherData.weather[0].description }}</p>
        <p>Sensação térmica: {{ Math.round(weatherData.main.feels_like) }}°C</p>
        <p>Umidade: {{ weatherData.main.humidity }}%</p>
        <p>Velocidade do vento: {{ weatherData.wind.speed }} m/s</p>
      </div>
    </main>

    <footer v-else>
      <p>Digite o nome da cidade e veja os dados sobre o clima!</p>
    </footer>
  </div>
</template>

<script>
import axios from "axios";

export default {
  data() {
    return {
      city: "",
      weatherData: null,
    };
  },
  methods: {
    async fetchWeather() {
      if (!this.city) return;
      const apiKey = "d5279129c31fa1556ad072072dc7c195";
      const url = `https://api.openweathermap.org/data/2.5/weather?q=${this.city}&units=metric&lang=pt_br&appid=${apiKey}`;
      try {
        const response = await axios.get(url);
        this.weatherData = response.data;
      } catch (error) {
        alert("Erro ao buscar previsões. Verifique o nome da cidade!");
        this.weatherData = null;
      }
    },
  },
};
</script>

<style>
.weather-app {
  font-family: "Arial", sans-serif;
  text-align: center;
  padding: 20px;
  color: #333;
}

header {
  margin-bottom: 20px;
}

input {
  padding: 10px;
  width: 250px;
  font-size: 16px;
  border: 1px solid #ccc;
  border-radius: 5px;
}

button {
  padding: 10px 20px;
  margin-left: 10px;
  font-size: 16px;
  background-color: #007bff;
  color: #fff;
  border: none;
  border-radius: 5px;
  cursor: pointer;
}

button:hover {
  background-color: #0056b3;
}

.weather-card {
  margin: 20px auto;
  padding: 20px;
  width: 300px;
  background: #f4f4f4;
  border-radius: 10px;
  box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
}

.weather-card h2 {
  margin: 0 0 10px;
}

.weather-card .temperature {
  font-size: 48px;
  margin: 10px 0;
}

footer p {
  font-size: 18px;
  color: #666;
}
</style>
