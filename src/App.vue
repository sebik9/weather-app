<template>
  <div id="app">
    <h1>Vue weather App</h1>
    <input class="userInput" v-model="city" placeholder=" Enter city...">
    <button @click="getweather">Submit</button>

    <div v-if="weather">
      <div class="app">
        <div class="leftSide">
          <div class="location">
            <h2 class="locCit">{{ weather.name }}</h2>
            <p class="date">{{ currentDate }}, {{ currentTime }}</p>
          </div>
          <div class="todayTempStatus">
            <p class="todayTemp">{{ weather.main.temp }}°C</p>
            <p class="todayStatus">{{ weather.weather[0].description }}</p>
          </div>
        </div>
        <div class="rightSide">
          <div class="upcoming">
            <div class="upcTop">
              <div v-if="upcoming && upcoming.list" class="segment1">
                <p>{{ upcomingDays[0] }}</p>
                <p><img :src="'http://openweathermap.org/img/w/' + upcoming.list[1].weather[0].icon + '.png'" alt=""></p>
                <p>{{ upcoming.list[1].weather[0].description }}</p>
                <p class="avgTemp">{{ upcoming.list[1].main.temp }}°C</p>
                <p>{{ upcoming.list[1].wind.speed }} km/h</p>
              </div>
              <div v-if="upcoming && upcoming.list" class="segment2">
                <p>{{ upcomingDays[1] }}</p>
                <p><img :src="'http://openweathermap.org/img/w/' + upcoming.list[2].weather[0].icon + '.png'" alt=""></p>
                <p>{{ upcoming.list[2].weather[0].description }}</p>
                <p class="avgTemp">{{ upcoming.list[2].main.temp }}°C</p>
                <p>{{ upcoming.list[2].wind.speed }} km/h</p>
              </div>
            </div>
            <div class="upcBot">
              <div v-if="upcoming && upcoming.list" class="segment1">
                <p>{{ upcomingDays[2] }}</p>
                <p><img :src="'http://openweathermap.org/img/w/' + upcoming.list[3].weather[0].icon + '.png'" alt=""></p>
                <p>{{ upcoming.list[3].weather[0].description }}</p>
                <p class="avgTemp">{{ upcoming.list[3].main.temp }}°C</p>
                <p>{{ upcoming.list[3].wind.speed }} km/h</p>
              </div>
              <div v-if="upcoming && upcoming.list" class="segment2">
                <p>{{ upcomingDays[3] }}</p>
                <p><img :src="'http://openweathermap.org/img/w/' + upcoming.list[4].weather[0].icon + '.png'" alt=""></p>
                <p>{{ upcoming.list[4].weather[0].description }}</p>
                <p class="avgTemp">{{ upcoming.list[4].main.temp }}°C</p>
                <p>{{ upcoming.list[4].wind.speed }} km/h</p>
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
            <h2 class="locCit">-,-</h2>
            <p class="date"> {{ currentDate }}, {{ currentTime }}</p>  
          </div>               
          <div class="todayTempStatus">
            <p class="todayTemp">-,-°C</p>   
            <p class="todayStaus">-,-</p>
          </div>
        </div>
        <div class="rightSide">
          <div class="upcoming">
            <div class="upcTop">
              <div class="segment1">
                <p>{{ upcomingDays[0] }}</p>
                <p>status icon</p>
                <p>status text</p>
                <p></p>
                <p>-,- km/h</p>
              </div>
              <div class="segment2">
                <p>{{ upcomingDays[1] }}</p>
                <p>status icon</p>
                <p>status text</p>
                <p>avg -,-°C</p>
                <p>-,- km/h</p>
              </div>     
            </div>
            <div class="upcBot">
              <div class="segment1">
                <p>{{ upcomingDays[2] }}</p>
                <p>status icon</p>
                <p>status text</p>
                <p>avg -,-°C</p>
                <p>-,- km/h</p>
              </div>
              <div class="segment2">
                <p>PH</p>
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

  </div>
</template>

<script>
export default {
  data() {
    return {
      city: "",
      weather: null,
      upcoming: null,
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
      const apiKey = "d467105a74a4fa1d4a6e9f7b760c3c0b"; 
      const apiUrl = `https://api.openweathermap.org/data/2.5/weather?q=${this.city}&appid=${apiKey}&units=metric`;

      
      fetch(apiUrl)
        .then(response => response.json())
        .then(data => {
          this.weather = data;
          this.getForecast(apiKey);
          this.showDate();        
        })
        .catch(error => {
          console.error("Error fetching weather data: ", error);
        });
        
    },

    getForecast(apiKey) {
      const forecastUrl = `https://api.openweathermap.org/data/2.5/forecast?q=${this.city}&appid=${apiKey}&units=metric`;

      fetch(forecastUrl)
        .then(response => response.json())
        .then(data => {
          this.upcoming = data;
          console.log(data)
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
      for (let i = 1; i <= 4; i++) {
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
    font-family: 'Roboto', sans-serif;
}

.app {
  width: 1000px;
  height: 600px;
  position: relative;
  display: flex;
  background-image: url(src/assets/cloudbg.jpg);
  background-repeat: no-repeat;
  background-size: 100%;
  color: white;
}

.todayTempStatus {
  position: relative;
  font-size: 30px;
  padding-top: 110px;
  padding-left: 50px;
  height: 60%;
  opacity: 0.6;
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
  height: 40%;
  opacity: 0.6;
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
}

.upcTop {
  display: flex;
  width: 100%;
  height: 50%;
}

.upcBot {
  display: flex;
  width: 100%;
  height: 50%;
}

.segment1 {
  width: 50%;
  height: 100%;
  text-align: center;
  padding-top: 90px;
  opacity: 0.6;
}

.segment2 {
  width: 50%;
  height: 100%;
  text-align: center;
  padding-top: 90px;
  opacity: 0.6;
}

.avgTemp {
  padding-top: 5px;
  padding-bottom: 5px;
}



</style>