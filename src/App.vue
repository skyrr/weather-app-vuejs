<template>
    <div id="app">
        <div class="weather">
            <div style="position: relative; width: 100%">
                <img src="../dist/cc_cloud_16x9 21342.png" style="opacity: 1; width: 100%; height: 100%" alt="">
                <div class="container" style="position: absolute; top: 1px; padding: -0.25rem">
                    <!--style="align-items: center; text-align: center;"-->
                    <div class="columns">
                        <div class="column is-one-third"></div>
                        <div class="column is-6" style="padding: -0.25rem">
                            <header class="page-header">
                                <div class="wrap">
                                    <h2 class="main-title title is-2">Weather</h2>
                                    <span class="caption" style="">Current weather and forecast for cities of Germany</span>
                                </div>
                            </header>
                        </div>
                        <div class="column is-one-third"></div>
                    </div>
                    <div class="columns">
                        <div class="column is-one-third"></div>
                        <div class="column is-6">
                            <suggestions
                                    ref="completor"
                                    v-model="searchQuery"
                                    :options="searchOptions"
                                    :onItemSelected="onSearchItemSelected"
                                    :onInputChange="onInputChange">
                                <div slot="item" slot-scope="props" class="single-item">
                                    <template v-if="">
                                    </template>
                                    <span class="name">{{props.item.name}}</span>
                                </div>
                            </suggestions>
                        </div>
                        <div class="column is-one-third"></div>
                    </div>
                </div>
                <div class="container">
                    <div class="columns">
                        <div class="column is-2">
                            <div class="column is-2" style="position: absolute; bottom: 35px">
                                <span class="weather-heading-temp">{{currentTemp}}&deg;</span><br>
                                <span class="weather-heading">{{city}}</span><br><br>
                                <div id="weatherCur1" class="sunCloudMain"><img src="../dist/suncloud.png" style="opacity: 0;width: 50%" alt=""></div>
                                <!--<img class="image1" src="" style="opacity: 0;width: 50%" alt=""><br>-->
                                <span class="weather-sign">humidity</span><br>
                                <span class="weather-heading">{{currentHumidity}}</span><br>
                                <span class="weather-sign">wind speed</span><br>
                                <span class="weather-heading">{{currentWind}}</span>
                            </div>
                        </div>
                        <div class="column is-2">
                            <div class="column is-2" style="position: absolute; bottom: 35px">
                                <span class="weather-sign">{{day1}}</span><br>
                                <span class="weather-heading">{{day1Temp}}&deg;</span><br>
                                <div id="weatherD1" class="sunCloudMain" style="height: 80px;background-size: 80px;"><img src="../dist/suncloud.png" style="opacity: 0;width: 50%" alt=""></div>
                                <span class="weather-sign">humidity</span><br>
                                <span class="weather-heading">{{day1Humidity}}</span><br>
                                <span class="weather-sign">wind speed</span><br>
                                <span class="weather-heading">{{day1Wind}}</span>
                            </div>
                        </div>
                        <div class="column is-2">
                            <div class="column is-2" style="position: absolute; bottom: 35px">
                                <span class="weather-sign">{{day2}}</span><br>
                                <span class="weather-heading">{{day2Temp}}&deg;</span><br>
                                <div id="weatherD2" class="sunCloudMain" style="height: 80px;background-size: 80px;"><img src="../dist/suncloud.png" style="opacity: 0;width: 50%" alt=""></div>
                                <span class="weather-sign">humidity</span><br>
                                <span class="weather-heading">{{day2Humidity}}</span><br>
                                <span class="weather-sign">wind speed</span><br>
                                <span class="weather-heading">{{day2Wind}}</span>
                            </div>
                        </div>
                        <div class="column is-2">
                            <div class="column is-2" style="position: absolute; bottom: 35px">
                                <span class="weather-sign">{{day3}}</span><br>
                                <span class="weather-heading">{{day3Temp}}&deg;</span><br>
                                <div id="weatherD3" class="sunCloudMain" style="height: 80px;background-size: 80px;"><img src="../dist/suncloud.png" style="opacity: 0;width: 50%" alt=""></div>
                                <span class="weather-sign">humidity</span><br>
                                <span class="weather-heading">{{day3Humidity}}</span><br>
                                <span class="weather-sign">wind speed</span><br>
                                <span class="weather-heading">{{day3Wind}}</span>
                            </div>
                        </div>
                        <div class="column is-2">
                            <div class="column is-2" style="position: absolute; bottom: 35px">
                                <span class="weather-sign">{{day4}}</span><br>
                                <span class="weather-heading">{{day4Temp}}&deg;</span><br>
                                <div id="weatherD4" class="sunCloudMain" style="height: 80px;background-size: 80px;"><img src="../dist/suncloud.png" style="opacity: 0;width: 50%" alt=""></div>
                                <span class="weather-sign">humidity</span><br>
                                <span class="weather-heading">{{day4Humidity}}</span><br>
                                <span class="weather-sign">wind speed</span><br>
                                <span class="weather-heading">{{day4Wind}}</span>
                            </div>
                        </div>
                        <div class="column is-2">
                            <div class="column is-2" style="position: absolute; bottom: 35px">
                                <span class="weather-sign">{{day5}}</span><br>
                                <span class="weather-heading">{{day5Temp}}&deg;</span><br>
                                <div id="weatherD5" class="sunCloudMain" style="height: 80px;background-size: 80px;"><img src="../dist/suncloud.png" style="opacity: 0;width: 50%" alt=""></div>
                                <span class="weather-sign">humidity</span><br>
                                <span class="weather-heading">{{day5Humidity}}</span><br>
                                <span class="weather-sign">wind speed</span><br>
                                <span class="weather-heading">{{day5Wind}}</span>
                            </div>
                        </div>
                        <!--<div class="column is-8">-->
                        <!--data-->
                        <!--<div class="column is-6">-->
                        <!--<pre>{{data}}</pre>-->
                        <!--</div>-->
                        <!--</div>-->
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
  import Suggestions from '../dist/v-suggestions'
  import '../dist/v-suggestions.css'

  import axios from 'axios'

  export default {
    name: 'app',
    data () {
      let countries = ['Afghanistan', 'Åland Islands', 'Albania', 'Algeria', 'American Samoa', 'AndorrA', 'Angola', 'Anguilla', 'Antarctica', 'Antigua and Barbuda', 'Argentina', 'Armenia', 'Aruba', 'Australia', 'Austria', 'Azerbaijan', 'Bahamas', 'Bahrain', 'Bangladesh', 'Barbados', 'Belarus', 'Belgium', 'Belize', 'Benin', 'Bermuda', 'Bhutan', 'Bolivia', 'Bosnia and Herzegovina', 'Botswana', 'Bouvet Island', 'Brazil', 'British Indian Ocean Territory', 'Brunei Darussalam', 'Bulgaria', 'Burkina Faso', 'Burundi', 'Cambodia', 'Cameroon', 'Canada', 'Cape Verde', 'Cayman Islands', 'Central African Republic', 'Chad', 'Chile', 'China', 'Christmas Island', 'Cocos (Keeling) Islands', 'Colombia', 'Comoros', 'Congo', 'Congo, The Democratic Republic of the', 'Cook Islands', 'Costa Rica', 'Cote D\'Ivoire', 'Croatia', 'Cuba', 'Cyprus', 'Czech Republic', 'Denmark', 'Djibouti', 'Dominica', 'Dominican Republic', 'Ecuador', 'Egypt', 'El Salvador', 'Equatorial Guinea', 'Eritrea', 'Estonia', 'Ethiopia', 'Falkland Islands (Malvinas)', 'Faroe Islands', 'Fiji', 'Finland', 'France', 'French Guiana', 'French Polynesia', 'French Southern Territories', 'Gabon', 'Gambia', 'Georgia', 'Germany', 'Ghana', 'Gibraltar', 'Greece', 'Greenland', 'Grenada', 'Guadeloupe', 'Guam', 'Guatemala', 'Guernsey', 'Guinea', 'Guinea-Bissau', 'Guyana', 'Haiti', 'Heard Island and Mcdonald Islands', 'Holy See (Vatican City State)', 'Honduras', 'Hong Kong', 'Hungary', 'Iceland', 'India', 'Indonesia', 'Iran, Islamic Republic Of', 'Iraq', 'Ireland', 'Isle of Man', 'Israel', 'Italy', 'Jamaica', 'Japan', 'Jersey', 'Jordan', 'Kazakhstan', 'Kenya', 'Kiribati', 'Korea, Democratic People\'S Republic of Korea, Republic of Kuwait', 'Kyrgyzstan', 'Lao People\'S Democratic Republic', 'Latvia', 'Lebanon', 'Lesotho', 'Liberia', 'Libyan Arab Jamahiriya', 'Liechtenstein', 'Lithuania', 'Luxembourg', 'Macao', 'Macedonia, The Former Yugoslav Republic of', 'Madagascar', 'Malawi', 'Malaysia', 'Maldives', 'Mali', 'Malta', 'Marshall Islands', 'Martinique', 'Mauritania', 'Mauritius', 'Mayotte', 'Mexico', 'Micronesia, Federated States of', 'Moldova, Republic of', 'Monaco', 'Mongolia', 'Montserrat', 'Morocco', 'Mozambique', 'Myanmar', 'Namibia', 'Nauru', 'Nepal', 'Netherlands', 'Netherlands Antilles', 'New Caledonia', 'New Zealand', 'Nicaragua', 'Niger', 'Nigeria', 'Niue', 'Norfolk Island', 'Northern Mariana Islands', 'Norway', 'Oman', 'Pakistan', 'Palau', 'Palestinian Territory, Occupied', 'Panama', 'Papua New Guinea', 'Paraguay', 'Peru', 'Philippines', 'Pitcairn', 'Poland', 'Portugal', 'Puerto Rico', 'Qatar', 'Reunion', 'Romania', 'Russian Federation', 'RWANDA', 'Saint Helena', 'Saint Kitts and Nevis', 'Saint Lucia', 'Saint Pierre and Miquelon', 'Saint Vincent and the Grenadines', 'Samoa', 'San Marino', 'Sao Tome and Principe', 'Saudi Arabia', 'Senegal', 'Serbia and Montenegro', 'Seychelles', 'Sierra Leone', 'Singapore', 'Slovakia', 'Slovenia', 'Solomon Islands', 'Somalia', 'South Africa', 'South Georgia and the South Sandwich Islands', 'Spain', 'Sri Lanka', 'Sudan', 'Suriname', 'Svalbard and Jan Mayen', 'Swaziland', 'Sweden', 'Switzerland', 'Syrian Arab Republic', 'Taiwan, Province of China', 'Tajikistan', 'Tanzania, United Republic of', 'Thailand', 'Timor-Leste', 'Togo', 'Tokelau', 'Tonga', 'Trinidad and Tobago', 'Tunisia', 'Turkey', 'Turkmenistan', 'Turks and Caicos Islands', 'Tuvalu', 'Uganda', 'Ukraine', 'United Arab Emirates', 'United Kingdom', 'United States', 'United States Minor Outlying Islands', 'Uruguay', 'Uzbekistan', 'Vanuatu', 'Venezuela', 'Viet Nam', 'Virgin Islands, British', 'Virgin Islands, U.S.', 'Wallis and Futuna', 'Western Sahara', 'Yemen', 'Zambia', 'Zimbabwe']
      return {
        items: [1, 2, 3, 4, 5],
        query: '',
        searchQuery: '',
        countries: countries,
        selectedCountry: null,
        selectedSearchItem: null,
        selectedSearchItem1: null,
        selectedSearchItem2: null,
        data: null,
        listitem: [1, 2, 3, 4, 5],
        list: null,
        city: null,
        currentTemp: null,
        dt: null,
        currentHumidity: null,
        currentWind: null,
        weatherSign: null,
        options: {},
        day1: null,
        day2: null,
        day3: null,
        day4: null,
        day5: null,
        day1Temp: null,
        day2Temp: null,
        day3Temp: null,
        day4Temp: null,
        day5Temp: null,
        day1Humidity: null,
        day2Humidity: null,
        day3Humidity: null,
        day4Humidity: null,
        day5Humidity: null,
        day1Wind: null,
        day2Wind: null,
        day3Wind: null,
        day4Wind: null,
        day5Wind: null,
        searchOptions: {
          placeholder: 'type city name',
          debounce: 400
        }
      }
    },
    mounted () {
      const urlLeipzig = `http://skyrr.space/api/weather/weather?city=6548737`
      function showIcon (cond, weatherCur) {
        switch (cond) {
          case 'clear sky':
            document.getElementById(weatherCur).className = 'sunMain'
              break
          case 'few clouds':
            document.getElementById(weatherCur).className = 'sunCloudMain'
              break
          case 'scattered clouds':
            document.getElementById(weatherCur).className = 'cloudMain'
              break
          case 'broken clouds':
            document.getElementById(weatherCur).className = 'cloudMain'
              break
          case 'mist':
            document.getElementById(weatherCur).className = 'cloudMain'
              break
          case 'shower rain':
            document.getElementById(weatherCur).className = 'rainMain'
              break
          case 'rain':
            document.getElementById(weatherCur).className = 'rainMain'
              break
          case 'thunderstorm':
            document.getElementById(weatherCur).className = 'rainMain'
              break
          case 'snow':
            document.getElementById(weatherCur).className = 'snowMain'
              break
          default:
            document.getElementById(weatherCur).className = 'cloudMain'
              break
        }
      }
      axios
        .get(urlLeipzig)
        .then(response => {
          this.city = response.data.name
          this.currentTemp = response.data.main.temp
          this.currentHumidity = response.data.main.humidity
          this.currentWind = response.data.wind.speed
          showIcon(response.data.weather.main, 'weatherCur1')
        })
      const urlLeipzigForecast = 'http://skyrr.space/api/weather/forecast?city=6548737'
      return new Promise(resolve => {
        let hour = 12
        let today = new Date()
        let tomorow = new Date(new Date().getTime() + 24 * 60 * 60 * 1000)
        this.day1 = tomorow.getDate() + '/' + tomorow.getMonth()
        let tomorow1 = new Date(new Date().getTime() + 2 * 24 * 60 * 60 * 1000)
        this.day2 = tomorow1.getDate() + '/' + tomorow1.getMonth()
        let tomorow2 = new Date(new Date().getTime() + 3 * 24 * 60 * 60 * 1000)
        this.day3 = tomorow2.getDate() + '/' + tomorow2.getMonth()
        let tomorow3 = new Date(new Date().getTime() + 4 * 24 * 60 * 60 * 1000)
        this.day4 = tomorow3.getDate() + '/' + tomorow3.getMonth()
        let tomorow4 = new Date(new Date().getTime() + 5 * 24 * 60 * 60 * 1000)
        this.day5 = tomorow4.getDate() + '/' + tomorow4.getMonth()
        // let image1 = document.getElementsByClassName("image1");
        axios.get(urlLeipzigForecast).then(response => {
          response.data.list.forEach((item) => {
          })
          for (let i = 0; i < 40; i++) {
            if (response.data.list[i].dt_txt.slice(8, 10) == tomorow.getDate()) {
              if (response.data.list[i].dt_txt.slice(11, 13) == hour) {
                this.day1 = response.data.list[i].dt_txt.slice(5, 10).replace('-', '/')
                this.day1Temp = response.data.list[i].main.temp
                this.day1Humidity = response.data.list[i].main.humidity
                this.day1Wind = response.data.list[i].wind.speed
                showIcon(response.data.list[i].weather[0].description, 'weatherD1')
              }
            }
            if (response.data.list[i].dt_txt.slice(8, 10) == tomorow1.getDate()) {
              if (response.data.list[i].dt_txt.slice(11, 13) == hour) {
                this.day2 = response.data.list[i].dt_txt.slice(5, 10).replace('-', '/')
                this.day2Temp = response.data.list[i].main.temp
                this.day2Humidity = response.data.list[i].main.humidity
                this.day2Wind = response.data.list[i].wind.speed
                showIcon(response.data.list[i].weather[0].description, 'weatherD2')
              }
            }
            if (response.data.list[i].dt_txt.slice(8, 10) == tomorow2.getDate()) {
              if (response.data.list[i].dt_txt.slice(11, 13) == hour) {
                this.day3 = response.data.list[i].dt_txt.slice(5, 10).replace('-', '/')
                this.day3Temp = response.data.list[i].main.temp
                this.day3Humidity = response.data.list[i].main.humidity
                this.day3Wind = response.data.list[i].wind.speed
                showIcon(response.data.list[i].weather[0].description, 'weatherD3')
              }
            }
            if (response.data.list[i].dt_txt.slice(8, 10) == tomorow3.getDate()) {
              if (response.data.list[i].dt_txt.slice(11, 13) == hour) {
                this.day4 = response.data.list[i].dt_txt.slice(5, 10).replace('-', '/')
                this.day4Temp = response.data.list[i].main.temp
                this.day4Humidity = response.data.list[i].main.humidity
                this.day4Wind = response.data.list[i].wind.speed
                showIcon(response.data.list[i].weather[0].description, 'weatherD4')
              }
            }
            if (response.data.list[i].dt_txt.slice(8, 10) == tomorow4.getDate()) {
              if (response.data.list[i].dt_txt.slice(11, 13) == hour) {
                this.day5 = response.data.list[i].dt_txt.slice(5, 10).replace('-', '/')
                this.day5Temp = response.data.list[i].main.temp
                this.day5Humidity = response.data.list[i].main.humidity
                this.day5Wind = response.data.list[i].wind.speed
                showIcon(response.data.list[i].weather[0].description, 'weatherD5')
              }
            }
          }
        })
      })
    },
    methods: {
      onInputChange (query) {
        if (query.trim().length === 0) {
          return null
        }
        const url = `http://skyrr.space/api/weather/deCity?city=${query}`
        return new Promise(resolve => {
          axios.get(url).then(response => {
            const items = []
            response.data.forEach((item) => {
              if (item.name) {
                items.push(item)
              }
            })
            resolve(items)
          })
        })
      },
      onSearchItemSelected (item) {
        const url1 = `http://skyrr.space/api/weather/forecast?city=${item.id}`
          function showIcon (cond, weatherCur) {
              switch (cond) {
                  case 'clear sky':
                      document.getElementById(weatherCur).className = 'sunMain'
                      break
                  case 'few clouds':
                      document.getElementById(weatherCur).className = 'sunCloudMain'
                      break
                  case 'scattered clouds':
                      document.getElementById(weatherCur).className = 'cloudMain'
                      break
                  case 'broken clouds':
                      document.getElementById(weatherCur).className = 'cloudMain'
                      break
                  case 'mist':
                      document.getElementById(weatherCur).className = 'cloudMain'
                      break
                  case 'shower rain':
                      document.getElementById(weatherCur).className = 'rainMain'
                      break
                  case 'rain':
                      document.getElementById(weatherCur).className = 'rainMain'
                      break
                  case 'thunderstorm':
                      document.getElementById(weatherCur).className = 'rainMain'
                      break
                  case 'snow':
                      document.getElementById(weatherCur).className = 'snowMain'
                      break
                  default:
                      document.getElementById(weatherCur).className = 'cloudMain'
                      break
              }
          }

          axios
            .get(url1)
            .then(response => (this.selectedSearchItem1 = response))
        this.selectedSearchItem1 = url1
        this.selectedSearchItem = item
        this.searchQuery = item.name
        this.city = item.name
        let hour = 12
        let today = new Date()
        let tomorow = new Date(new Date().getTime() + 24 * 60 * 60 * 1000)
        this.day1 = tomorow.getDate() + '/' + tomorow.getMonth()
        let tomorow1 = new Date(new Date().getTime() + 2 * 24 * 60 * 60 * 1000)
        this.day2 = tomorow1.getDate() + '/' + tomorow1.getMonth()
        let tomorow2 = new Date(new Date().getTime() + 3 * 24 * 60 * 60 * 1000)
        this.day3 = tomorow2.getDate() + '/' + tomorow2.getMonth()
        let tomorow3 = new Date(new Date().getTime() + 4 * 24 * 60 * 60 * 1000)
        this.day4 = tomorow3.getDate() + '/' + tomorow3.getMonth()
        let tomorow4 = new Date(new Date().getTime() + 5 * 24 * 60 * 60 * 1000)
        this.day5 = tomorow4.getDate() + '/' + tomorow4.getMonth()
        const url2 = `http://skyrr.space/api/weather/weather?city=${item.id}`
        axios
          .get(url2)
          .then(response => {
            this.currentTemp = response.data.main.temp
            this.currentHumidity = response.data.main.humidity
            this.currentWind = response.data.wind.speed
          })
        return new Promise(resolve => {
          axios.get(url1).then(response => {
            response.data.list.forEach((item) => {
            })
            for (let i = 0; i < 40; i++) {
              if (response.data.list[i].dt_txt.slice(8, 10) == tomorow.getDate()) {
                if (response.data.list[i].dt_txt.slice(11, 13) == hour) {
                  this.day1 = response.data.list[i].dt_txt.slice(5, 10).replace('-', '/')
                  this.day1Temp = response.data.list[i].main.temp
                  this.day1Humidity = response.data.list[i].main.humidity
                  this.day1Wind = response.data.list[i].wind.speed
                  showIcon(response.data.list[i].weather[0].description, 'weatherD1')
                }
              }
              if (response.data.list[i].dt_txt.slice(8, 10) == tomorow1.getDate()) {
                if (response.data.list[i].dt_txt.slice(11, 13) == hour) {
                  this.day2 = response.data.list[i].dt_txt.slice(5, 10).replace('-', '/')
                  this.day2Temp = response.data.list[i].main.temp
                  this.day2Humidity = response.data.list[i].main.humidity
                  this.day2Wind = response.data.list[i].wind.speed
                  showIcon(response.data.list[i].weather[0].description, 'weatherD2')
                }
              }
              if (response.data.list[i].dt_txt.slice(8, 10) == tomorow2.getDate()) {
                if (response.data.list[i].dt_txt.slice(11, 13) == hour) {
                  this.day3 = response.data.list[i].dt_txt.slice(5, 10).replace('-', '/')
                  this.day3Temp = response.data.list[i].main.temp
                  this.day3Humidity = response.data.list[i].main.humidity
                  this.day3Wind = response.data.list[i].wind.speed
                  showIcon(response.data.list[i].weather[0].description, 'weatherD3')
                }
              }
              if (response.data.list[i].dt_txt.slice(8, 10) == tomorow3.getDate()) {
                if (response.data.list[i].dt_txt.slice(11, 13) == hour) {
                  this.day4 = response.data.list[i].dt_txt.slice(5, 10).replace('-', '/')
                  this.day4Temp = response.data.list[i].main.temp
                  this.day4Humidity = response.data.list[i].main.humidity
                  this.day4Wind = response.data.list[i].wind.speed
                  showIcon(response.data.list[i].weather[0].description, 'weatherD4')
                }
              }
              if (response.data.list[i].dt_txt.slice(8, 10) == tomorow4.getDate()) {
                if (response.data.list[i].dt_txt.slice(11, 13) == hour) {
                  this.day5 = response.data.list[i].dt_txt.slice(5, 10).replace('-', '/')
                  this.day5Temp = response.data.list[i].main.temp
                  this.day5Humidity = response.data.list[i].main.humidity
                  this.day5Wind = response.data.list[i].wind.speed
                  showIcon(response.data.list[i].weather[0].description, 'weatherD5')
                }
              }
            }
          })
        })
      }
    },
    components: {
      Suggestions
    }
  }
</script>

<style>

  .page-header {
    height: 150px;
    margin-bottom: 0.5rem;
    display: flex;
    align-items: center;
    text-align: center;
  }

  .page-header .wrap {
    margin: 0 auto;
    width: 100%;
    color: #f8f8f8;
  }

  .page-header .wrap .main-title {
    color: #f8f8f8;
    font-weight: bold;
    text-shadow: 1px 1px 1px #333;
  }

  .title {
    font-weight: normal;
  }

  .completor .item {

  }

  .single-item {
    overflow: hidden;
    position: relative;
  }

  .image-wrap {
    width: 80px;
    height: 80px;
    background-position: center;
    -webkit-background-size: cover;
    background-size: cover;
    float: left;
    margin-right: 1em;
  }

  .single-item .name {
    /*float: left;*/
  }

  #app .token.number {
    font-size-adjust: none;
    -webkit-box-align: center;
    -ms-flex-align: center;
    align-items: center;
    background-color: #f5f5f5;
    border-radius: 290486px;
    display: -webkit-inline-box;
    display: -ms-inline-flexbox;
    display: inline-flex;
    font-size: 1em;
    height: auto;
    -webkit-box-pack: center;
    -ms-flex-pack: center;
    justify-content: center;
    margin-right: 0rem;
    min-width: auto;
    padding: 0rem;
    text-align: center;
    vertical-align: top;
  }

  #app {
    padding-bottom: 1px;
  }

  #app code[class*=language-], pre[class*=language-] {
    font-family: monospace;
  }
  /*.weather {*/
    /*background-image: url('../dist/cc_cloud_16x9 2134.png');*/
    /*background-repeat: no-repeat;*/
  /*}*/
  .weather-heading{
      color: white;
      font-size: 30px;
  }
  .weather-heading-temp{
      color: white;
      font-size: 56px;
  }
  .weather-sign{
      color: white;
      font-size: 16px;
  }
  .paramsCur {
      height: 100px;
      background-size: 100px;
  }
  .paramsForecast {
      height: 80px;
      background-size: 80px;
  }
  .sunCloudMain {
      background-image: url("../dist/suncloud.png");
      background-repeat: no-repeat;
      height: 100px;
      background-size: 100px;
  }
  .rainMain {
      background-image: url("../dist/rain.png");
      background-repeat: no-repeat;
      height: 100px;
      background-size: 100px;
  }
  .snowMain {
      background-image: url("../dist/snow.png");
      background-repeat: no-repeat;
      height: 100px;
      background-size: 100px;
  }
  .sunMain {
      background-image: url("../dist/sun.png");
      background-repeat: no-repeat;
      height: 100px;
      background-size: 100px;
  }
  .cloudMain {
      background-image: url("../dist/cloud.png");
      background-repeat: no-repeat;
      height: 100px;
      background-size: 100px;
  }
</style>
