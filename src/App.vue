<template>
  <div class="weather" :class="weatherClass">
    <div class="container">

      <div class="card weather-form">
        <input type="text" 
          class="weather-form-input"
          placeholder="Enter city"
          v-model="searchQuery">
        <button class="weather-form-btn" @click="weatherSearch">Search</button>        
      </div>

      <div class="card weather-load" v-if="loading">Loading...</div>

      <div class="weather-info" 
        v-show="!error && location && temperature !== null && description">
          
          <div class="card" v-if="error">Error</div>

          <div class="weather-info-text">
            <p class="card">{{ location }}</p>
            <p class="card">{{ temperature }}°C</p>
            <p class="card">{{ description }}</p>
          </div>
      </div>
      
      <div class="weather-bg">
        <div>
          <img class="weather-bg-img bg" src="./assets/bg.jpg" alt="App Background">
          <img class="weather-bg-img sunny" src="./assets/sunny.jpg" alt="Sunny">
          <img class="weather-bg-img overcast" src="./assets/overcast.jpg" alt="Overcast">
          <img class="weather-bg-img partly-cloudy" src="./assets/partly-cloudy.jpg" alt="Partly-Cloudy">
        </div>
      </div>

    </div>
  </div>
</template>

<script>
export default{
  data(){
    return{
      location: '',
      temperature: null,
      description: '',
      loading: false,
      error: false,
      searchQuery: ''
    }
  },
  methods: {
    weatherSearch(){
      this.loading = true;
      this.error = false;
      fetch(`http://api.weatherapi.com/v1/current.json?key=48c029b61f30419ab3372937250402&q=${this.searchQuery}`)
        .then(response => response.json())
        .then(data => {
          this.loading = false;
          this.location = data.location.name;
          this.temperature = data.current.temp_c;
          this.description = data.current.condition.text;
          this.resetSearchQuery();
        })
      .catch(error => {
        this.loading = false;
        this.error = true;
        console.log(error);
      })
    },
    resetSearchQuery(){
      this.searchQuery = '';
    }
  },
  computed: {
    weatherClass(){
      if(this.description.includes('Sunny')){
        return 'sunny';
      } else if(this.description.includes('Overcast')) {
        return 'overcast';
      } else if(this.description.includes('Partly cloudy')) {
        return 'partly-cloudy';
      } else {
        return '';
      }
    }
  }
}
</script>