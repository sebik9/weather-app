<template>
  <div id="app">
    <h1>Vue weather App</h1>
    <input class="userInput" v-model="city" placeholder=" Enter city...">
    <button @click="getweather">Submit</button>

    <div v-if="weather">
      <div class="app">
        <div class="leftSide">
          <div class="location">
            <h2 class="locCit">{{ weather.location.name }}</h2>
            <p class="date"> {{ currentDate }}, {{ currentTime }}</p>  
          </div>               
          <div class="todayTempStatus">
            <p class="todayTemp">{{ weather.current.temp_c }}°C</p>   
            <p class="todayStaus">{{ weather.current.condition.text }}</p>
          </div>
        </div>
        <div class="rightSide">
          <div class="upcoming">
            <div class="upcTop">
              <div class="segment1">
                <p>{{ upcomingDays[0] }}</p>
                <p>status icon</p>
                <p>status text</p>
                <p>temp</p>
                <p>wind info</p>
              </div>
              <div class="segment2">
                <p>{{ upcomingDays[1] }}</p>
                <p>status icon</p>
                <p>status text</p>
                <p>temp</p>
                <p>wind info</p>
              </div>     
            </div>
            <div class="upcBot">
              <div class="segment1">
                <p>{{ upcomingDays[2] }}</p>
                <p>status icon</p>
                <p>status text</p>
                <p>temp</p>
                <p>wind info</p>
              </div>
              <div class="segment2">
                <p>Scrap</p>
                <p>PH</p>
                <p>PH</p>
                <p>PH</p>
                <p>PH</p>
              </div>  
            </div> 
          </div> 
        </div>
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
            <div class="upcTop">
              <div class="segment1">
                <p>Day 1</p>
                <p>status icon</p>
                <p>status text</p>
                <p>temp</p>
                <p>wind info</p>
              </div>
              <div class="segment2">
                <p>Day 2</p>
                <p>status icon</p>
                <p>status text</p>
                <p>temp</p>
                <p>wind info</p>
              </div>     
            </div>
            <div class="upcBot">
              <div class="segment1">
                <p>Day 3</p>
                <p>status icon</p>
                <p>status text</p>
                <p>temp</p>
                <p>wind info</p>
              </div>
              <div class="segment2">
                <p>Day 4</p>
                <p>status icon</p>
                <p>status text</p>
                <p>temp</p>
                <p>wind info</p>
              </div>  
            </div> 
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
      forecast: null,
      currentDate: "",
      currentTime: "",
      dayString: "",
      upcomingDays: [],
    };
  },
  mounted() {
    this.showDate();
  },
  methods: {
    getweather() {
      const apiKey = "8eb4612660944712b23103312232610"; 
      const apiUrl = `https://api.weatherapi.com/v1/current.json?key=${apiKey}&q=${this.city}`;

      
      fetch(apiUrl)
        .then(response => response.json())
        .then(data => {
          this.weather = data;
        })
        .catch(error => {
          console.error("Error fetching weather data: ", error);
        });

        this.getForecast(apiKey);
    },

    getForecast(apiKey) {
      const forecastUrl = `https://api.weatherapi.com/v1/forecast.json?key=${apiKey}&q=${this.city}&days=3`;

      fetch(forecastUrl)
        .then(response => response.json())
        .then(data => {
          this.forecast = data;
        })
        .catch(error => {
          console.error("Error fetching forecast", error);
          });

    },

    showDate() {
      const currentDate = new Date();

      const dayNames = [
        "Sunday", "Monday", "Tuesday", "Wednesday",
        "Thursday", "Friday", "Saturday"
      ];

      const monthNames = [
        "January", "February", "March", "April",
        "May", "June", "July", "August",
        "September", "October", "November", "December"
      ];

      // Get the current date
      const dayIndex = currentDate.getDay();
      const day = currentDate.getDate();
      const monthIndex = currentDate.getMonth(); 

      // Get the current time
      const hours = currentDate.getHours();
      const minutes = currentDate.getMinutes();

      const today = new Date().getDay();
      const upcomingDays = [];
      for (let i = 1; i <= 3; i++) {
      const nextDayIndex = (today + i) % 7;
      upcomingDays.push(dayNames[nextDayIndex]);
    }

      
      this.currentDate = `${day}, ${monthNames[monthIndex]}`;
      this.currentTime = `${hours}:${minutes}`;
      this.dayString = `${dayNames[dayIndex]}`;
      this.upcomingDays = upcomingDays
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
  width: 100%;
  height: 100%;
}

.leftSide {
  width: 40%;
  height: 100%;
}

.rightSide {
  width: 60%;
  background-color: blue;
}

.upcTop {
  display: flex;
  width: 100%;
  height: 50%;
  background-color: yellow;
}

.upcBot {
  display: flex;
  width: 100%;
  height: 50%;
  background-color: green;
}

.segment1 {
  width: 50%;
  height: 100%;
  text-align: center;
  padding-top: 100px;
}

.segment2 {
  width: 50%;
  height: 100%;
  text-align: center;
  padding-top: 100px;
}

</style>