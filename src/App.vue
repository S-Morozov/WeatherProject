<script>

import axios from 'axios';


export default {
    
    data() {
        return {
            city: "",
            error: "",
            info: null
        };
    }, 
    computed: {
        cityName() {
            return this.city 
        },
        showTemp() {
            return "Temperature is: " + Math.round(this.info.main.temp) + "°C"
        
        },

        showFeelsLike() {
            return "Feels like: " + Math.round(this.info.main.feels_like) +  "°C"
        }
    },

    methods: {
        getWeather() {
            if(this.city.trim().length < 2) {
                this.error = "You need to use more than 1 symbol"
                return false
            }

            this.error = '';
            axios.get(`https://api.openweathermap.org/data/2.5/weather?q=${this.city}&units=metric&appid=6a5c2f4415c5bfe91a212b4cd44e4207`)
                .then(res => {
                this.info = res.data; 
            })
                .catch(error => {
                console.error(error); 
                this.error = "City not found"; 
    });
        }
    }

};

</script>

<template>
    <div class="wrapper">
        <h1>Weather Project on Vue.js </h1>
        <p>Current weather in {{ city == "" ? "your city": cityName}}</p>

        <input type="text" v-model="city" placeholder="Enter your city: ">

        <button v-if="city !='' " @click="getWeather()" >Get result</button>
        <button disabled v-else>Enter city!</button>
        <p class = 'error'>{{ error }}</p>

        <div v-if="info != null">
            <p>{{showTemp}}</p>
            <p>{{showFeelsLike}}</p>
        </div>
    </div>
  
</template>

<style scoped>





.error {
    color: darkred;
}

.wrapper {
    width: 900px;
    height: 500px;
    border-radius: 50px;
    background: rgb(30, 30, 210);
    text-align: center;
    color: whitesmoke
}

.wrapper h1{
    padding-top: 50px;
}

.wrapper p {
    margin-top: 20px;
}

.wrapper input {
    margin-top: 30px;
    background: transparent;
    border: 0;
    border-bottom: 2px solid darkslateblue;
    color: whitesmoke;
    font-size: 14px;
    padding: 5px 8px;
    outline: none;
    transition: border-bottom-color 0.3s ease;
}

.wrapper input:focus{
    border-bottom-color: crimson;
}

.wrapper button:disabled {
    background: darkmagenta;
    cursor: not-allowed;
}

.wrapper button {
    width: 100px;
    height: 30px;
    background: crimson;
    color: aliceblue;
    border-radius: 10px;
    border: 1px solid whitesmoke;
    margin-left: 20px;
    cursor: pointer;
    transition: transform 500ms ease;

}

.wrapper button:hover {
    transform: scale(1.1) translateY(-5px);
}

</style>
