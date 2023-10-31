<template>
  <div id="app">
    <h1>Vue weather App</h1>
    <input class="userInput" v-model="city" placeholder=" Enter city...">
    <button @click="getweather">Submit</button>

    <div v-if="weather">
      <h2>current weather in {{ weather.location.name }}, {{ weather.location.country }}</h2>
      <div class="top-bar">
        <div class="top-left">
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

    <div v-else>

      <div class="app">
        <div class="leftSide">
          <div class="location">
            <h2 class="locCit">City</h2>
            <p class="date"> {{ currentDate }}, {{ currentTime }}</p>  
          </div>               
          <div class="todayTempStatus">
            <p class="todayTemp">20°C</p>   
            <p class="todayStaus">Broken clouds</p>
          </div>
        </div>
        <div class="rightSide">
          <div class="upcoming">
            <div class="">Information 1</div>
            <div class="">Information 2</div>          
            <div class="">Information 3</div>
            <div class="">Information 4</div>   
          </div> 
        </div>
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
      currentDate: "",
      currentTime: "",
    };
  },
  mounted() {
    this.showDate();
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
    showDate() {
      const currentDate = new Date();

      const monthNames = [
        "January", "February", "March", "April",
        "May", "June", "July", "August",
        "September", "October", "November", "December"
      ];

      // Get the current date
      const day = currentDate.getDate();
      const monthIndex = currentDate.getMonth(); 

      // Get the current time
      const hours = currentDate.getHours();
      const minutes = currentDate.getMinutes();

      
      this.currentDate = `${day}, ${monthNames[monthIndex]}`;
      this.currentTime = `${hours}:${minutes}`;
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

.app {
  background-color: aqua;
  width: 1000px;
  height: 600px;
  position: relative;
  display: flex;
}

.todayTempStatus {
  position: relative;
  font-size: 30px;
  padding-top: 110px;
  padding-left: 50px;
  background-color: brown; 
  height: calc(100% - 181px);
}

.userInput {
  width: 200px;
  height: 30px;  
  border: 1px;
  border-style: solid;
}

.location {
  position: relative;
  padding-left: 50px;
  padding-top: 90px;
  opacity: 0.6;
  color: black;
  background-color: blueviolet;
}

.date {
  padding-top: 10px;
  font-size: 20px;
}

.locCit {
  font-size: 50px;
}

.todayTemp {
  font-size: 70px;
}

.todayStaus {
  font-size: 30px;
}

.upcoming {
  position: relative;
}

.leftSide {
  width: 40%;
  height: 100%;
}

.rightSide {
  width: 60%;
  background-color: blue;
}
</style>