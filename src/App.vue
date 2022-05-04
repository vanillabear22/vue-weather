<template>
    <div id="app">
        <main>
            <div class="search-box">
                <input type="text"
                       class="search-bar"
                       placeholder="Search..."
                       v-model="query"
                       @keypress="fetchWeather"
                >
            </div>
            <div class="weather-wrap" v-if="typeof weather.main !='undefined'">
                <div class="location-box">
                    <div class="location">{{weather.name}}, {{weather.sys.country}}</div>
                    <div class="date">{{getDate()}}</div>
                </div>
                <div class="weather-box">
                    <div class="temp">{{Math.round(weather.main.temp)}}℃</div>
                    <div class="weather">{{weather.weather[0].main}}</div>
                </div>
            </div>
        </main>
    </div>
</template>

<script>


    export default {
        name: 'App',
        data() {
            return {
                api_key: 'ee0318aa9ab5919b20cd4d8a9ec62171',
                url_base: 'https://api.openweathermap.org/data/2.5/',
                query: '',
                weather: {},
                date: ''
            }
        },
        methods: {
            fetchWeather(e) {
                if (e.key == "Enter") {
                    fetch(`${this.url_base}weather?q=${this.query}&units=metric&APPID=${this.api_key}`)
                        .then(res => {
                            return res.json();
                        }).then(this.setResults)
                }
            },
            setResults(results) {
                this.weather = results;
                document.getElementById("app").style.backgroundImage =
                    `url("./img/${this.weather.weather[0].main}.jpg")`;
            },
            getDate() {
                let d = new Date();
                let months = ["January", "February", "March", "April", "May", "June", "July",
                    "August", "September", "October", "November", "December"];
                let days = ["Sunday", "Monday", "Tuesday", "Wednesday", "Thursday", "Friday", "Saturday"];
                let day = days[d.getDay()];
                let date = d.getDate();
                let month= months[d.getMonth()];
                let year = d.getFullYear();
                return `${day} ${date} ${month} ${year}`;
            }
        }
    }
</script>

<style lang="scss">
    * {
        margin: 0;
        padding: 0;
        box-sizing: border-box;
    }

    body {
        font-family: 'montserrat', sans-serif;
        background-image: url("../public/img/Clear.jpg");
        background-size: cover;
    }

    #app {
        background-size: cover;
        background-position: bottom;
        transition: .4s;
    }

    main {
        min-height: 100vh;
        padding: 25px;
        background-image: linear-gradient(to bottom, rgba(0, 0, 0, .2), rgba(0, 0, 0, .7));
        text-align: center;
    }


    .search-box {
        display: inline-block;
        .search-bar {
            width: 100%;
            margin-bottom: 30px;
            display: block;
            padding: 15px;
            color: #313131;
            font-size: 20px;

            appearance: none;
            border: none;
            outline: none;
            background: rgba(255, 255, 255, .5);
            box-shadow: 0 0 16px rgba(0, 0, 0, .25);
            border-radius: 2px 16px 2px 16px;
            transition: .4s;
        }

        .search-bar:focus {
            background: rgba(255, 255, 255, .75);
        }

    }

    .location-box {
        .location {
            color: #fff;
            font-size: 32px;
            font-weight: 500;
            text-align: center;
            text-shadow: 1px 3px rgba(0, 0, 0, .25);
        }

        .date {
            color: #fff;
            font-size: 20px;
            font-weight: 300;
            font-style: italic;
            text-align: center;
        }

    }

    .weather-box {
        text-align: center;
        text-shadow: 3px 6px rgba(0, 0, 0, .25);

        .temp {
            color: #fff;
            font-size: 50px;
            font-weight: 900;
            padding: 10px 30px;
            background: rgba(255, 255, 255, .2);
            display: inline-block;
            box-shadow: 3px 6px rgba(0, 0, 0, .25);
            margin: 30px 0;
            border-radius: 15px;
        }

        .weather {
            color: #fff;
            font-size: 48px;
            font-weight: 700;
            font-style: italic;

        }
    }
</style>
