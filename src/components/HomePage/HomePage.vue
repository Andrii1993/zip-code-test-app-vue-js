<template xmlns="http://www.w3.org/1999/html">
  <div class="container">
    <h1>Search by US zip code</h1>
    <form @submit.prevent="searchZipCode">
      <div class="input-group input-group-sm mb-3">
        <span class="input-group-text text" id="inputGroup-sizing-sm">Enter your ZIP code:</span>
        <input type="text" class="form-control" id="zip-code" v-model="zipCode" required>
      </div>
      <div class="btn__container">
        <button type="submit" class="btn btn-primary">Search</button>
        <button type="button" class="btn btn-secondary" @click="reset">Home Page</button>
        <button type="button" class="btn btn-info" @click="searchIp">IP-adress</button>
      </div>
    </form>
    <div v-if="showResults" class="result primary">
      <h2>Search results</h2>
      <div class="result__code">
        <p><i>State: </i>{{ state }}</p>
        <p><i>City: </i>{{ city }}</p>
      </div>
    </div>
  </div>
  <div v-if="ipAddress" class="result result_ip container">
    <h2>IP-adress</h2>
    <div class="result__info">
      <p><i>IP-adress: </i>{{ ipAddress }}</p>
      <p><i>Provider: </i>{{ ipProvider }}</p>
      <p><i>City: </i>{{ ipCity }}</p>
      <p><i>Region: </i>{{ ipRegion }}</p>
      <p><i>Country: </i>{{ ipCountry }}</p>
    </div>
  </div>
</template>
<script>
export default {
  data() {
    return {
      zipCode: '',
      state: '',
      city: '',
      showResults: false,
      ipAddress: '',
      ipProvider: '',
      ipCity: '',
      ipRegion: '',
      ipCountry: ''
    }
  },
  methods: {
    async searchZipCode() {
      const apiKey = '<your_api_key_here>'
      const url = `https://api.zip-codes.com/ZipCodesAPI.svc/1.0/QuickGetZipCodeDetails/${this.zipCode}?key=${apiKey}`
      const response = await fetch(url)
      const data = await response.json()
      if (data.City) {
        this.state = data.State
        this.city = data.City
        this.showResults = true
      } else {
        alert('Поштовий індекс не знайдено')
      }
    },
    async searchIp() {
      const url = 'https://api.ipify.org/?format=json'
      const response = await fetch(url)
      const data = await response.json()
      this.ipAddress = data.ip

      // Використовуємо GeoIP API для отримання додаткової інформації про IP-адресу
      const geoIpUrl = 'https://ipapi.co/' + this.ipAddress + '/json/'
      const geoIpResponse = await fetch(geoIpUrl)
      const geoIpData = await geoIpResponse.json()

      // Виводимо інформацію про IP-адресу
      this.ipProvider = geoIpData.org
      this.ipCity = geoIpData.city
      this.ipRegion = geoIpData.region
      this.ipCountry = geoIpData.country_name
    },

    reset() {
      this.showResults = false
      this.zipCode = ''
    }
  }
}
</script>
<style scoped>
.container, .input-group, .result, .btn__container {
  margin-top: 25px;
}

.btn-secondary {
  margin: 0 20px;
}

.result {
  border: 1px solid cornflowerblue;
  border-radius: 15px;
  padding-top: 10px;
}

.result__info {
  display: flex;
  justify-content: space-between;
}

.result__code {
  display: flex;
  justify-content: space-between;
  padding: 0 50px;
}

i {
  font-weight: 700;
  font-family: Roboto, sans-serif;
  font-size: 20px;
  color: ghostwhite;
}

p {
  font-weight: 700;
  font-family: Roboto, sans-serif;
  font-size: 20px;
  color: ghostwhite;
}

span,input  {
  font-weight: 700;
  font-family: Roboto, sans-serif;
  line-height: 40px;
  color: #2c3e50;
}

h2 {
  font-weight: 700;
  font-family: Roboto, sans-serif;
  font-size: 32px;
  color: ghostwhite;
}

h1 {
  font-weight: 700;
  font-family: Roboto, sans-serif;
  font-size: 50px;
  color: ghostwhite;
}
</style>