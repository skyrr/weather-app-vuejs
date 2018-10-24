<template>
  <div id="app">
    <div class="foto">
      <header class="page-header">
      <div class="wrap">
          <h2 class="main-title title is-2">Weather application</h2>
          <span class="caption">Current weather and forecast for cities in Germany</span>
        </div>
    </header>
      <div class="container">
      <div class="columns">
        <div class="column is-8 is-offset-2">
          <h5 class="title is-5">Start typing city name</h5>
          <div class="columns">
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
            <div class="column is-6">
              <pre>{{selectedSearchItem}}</pre>
            </div>
          </div>
          <!--<div class="column is-6">-->
          <!--<pre>{{list}}</pre>-->
          <!--</div>-->
          humidity
          <div class="column is-6">
            <pre>{{humidity}}</pre>
          </div>
          temp
          <div class="column is-6">
            <pre>{{temp}}</pre>
          </div>
          data
          <div class="column is-6">
            <pre>{{data}}</pre>
          </div>
          <!--<div class="column is-6">-->
            <!--<pre>{{selectedSearchItem1.data}}</pre>-->
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
        data: null,
        listitem: [1, 2, 3, 4, 5],
        list: null,
        temp: null,
        dt: null,
        humidity: null,
        options: {},
        searchOptions: {
          placeholder: 'type city name',
          debounce: 400
        }
      }
    },
    mounted () {
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
        axios
            .get(url1)
            .then(response => (this.selectedSearchItem1 = response))
        this.selectedSearchItem1 = url1
        this.selectedSearchItem = item
        return new Promise(resolve => {
          axios.get(url1).then(response => {
            // const dts = []
            // console.log(response.data.list.items)
            response.data.list.forEach((item) => {
              if (item.dt) {
                console.log(item)
                this.dt = item
              }
            })
            // this.data = response.data
            this.data = response.data.list[0]
            this.humidity = response.data.list[0].main.humidity
            this.temp = response.data.list[0].main.temp
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
    height: 300px;
    margin-bottom: 2rem;
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
  .foto {
    background-image: url('../dist/cc_cloud_16x9 2134.png');
    background-repeat: no-repeat;
  }
</style>
