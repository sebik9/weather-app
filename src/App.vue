<template>
  <div id="app">
    <h1>Vue weather App</h1>
    <input class="userInput" v-model="city" placeholder=" Enter city...">
    <button @click="getweather">Submit</button>
    <div v-if="weather">
      <h2>current weather in {{ weather.location.name }}, {{ weather.location.country }}</h2>
      <div class="top-bar">
        <div class="top-left">
          <img class="icon" :src="weather.current.condition.icon" alt="weather icon">
          <p class="currentTemp">{{ weather.current.temp_c }}°C</p>
        </div>

        <div class="top-right">

            <div class="column">
              <div class="info-box">Information 1</div>
              <div class="info-box">Information 2</div>
            </div>
          <div class="column">
              <div class="info-box">Information 3</div>
              <div class="info-box">Information 4</div>
          </div>

        </div>
      </div>
        <div class="other-temp">
          <p>weather: {{ weather.current.condition.text }}</p>
          <p>Humidity: {{ weather.current.humidity }}%</p>
          
        </div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      city: "",
      weather: null,
    };
  },
  methods: {
    getweather() {
      const apiKey = "8eb4612660944712b23103312232610"; // 
      const apiUrl = `https://api.weatherapi.com/v1/current.json?key=${apiKey}&q=${this.city}`;

      // Using fetch for the HTTP request
      fetch(apiUrl)
        .then(response => response.json())
        .then(data => {
          this.weather = data;
        })
        .catch(error => {
          console.error("Error fetching weather data: ", error);
        });
    },
  },
};
</script>

<style scoped>

* {
    margin:0;
    padding:0;
    box-sizing: border-box;
}

.currentTemp {
  font-size: 30px;
}

.userInput {
  width: 200px;
  height: 30px;  
  border: 1px;
  border-style: solid;
}

.top-bar{
  display: flex;
  position: relative;
  width: 500px;
  height: 100px;
  background-color: red;
}

.top-left {
  background-color: yellow;
  position: relative;
  width: 50%;
  height: 100%;
  text-align: center;
}

.top-right {
  position: relative;
  width: 50%;
  height: 100%;
  display: flex;
  text-align: center;
  justify-content: center;
}

.column {
  width: 50%;
  height: 50%;
  position: relative;
  }

.info-box {
  position: relative;
  width: 100%;
  height: 100%;
  text-align: center;
  padding-top: 13px;
}
</style>