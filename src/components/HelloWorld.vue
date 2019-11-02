<template>
  <div class="hello">
    <h1>{{ cityName }}에서 오늘 모입지?</h1>
    <div>Lat: {{ lat }}</div>
    <div>Lon: {{ lon }}</div>
    <div>cityName: {{ cityName }}</div>
    <div>temperature: {{ temperature }}</div>
    <div>clothes: {{ clothes }}</div>
  </div>
</template>

<script>
export default {
  name: "HelloWorld",
  props: {
    msg: String,
    lat: Number,
    lon: Number,
    cityName: String,
    temperature: String,
  },
  created: function () {
    console.log("created");

    navigator.geolocation.getCurrentPosition((position) => {
      this.lat = position.coords.latitude
      this.lon = position.coords.longitude
      this.fetchWeather(this.lat, this.lon);
    });
  },
  computed: {
    clothes: function () {
      const temp = this.temperature;
      console.log("컴퓨티드", temp, temp < 4)

      if (temp >= 28) {
        return "민소매, 반팔, 반바지, 짧은 치마, 린넨 옷";
      }
      if (temp >= 23) {
        return "반팔, 얇은 셔츠, 반바지, 면바지";
      }
      if (temp >= 20) {
        return "블라우스, 긴팔 티, 면바지, 슬랙스, ";
      }
      if (temp >= 17) {
        return "얇은 셔츠나 가디건/니트, 맨투맨, 후드, 긴 바지";
      }
      if (temp >= 12) {
        return "자켓, 가디건, 청자켓, 니트, 스타킹, 청바지";
      }
      if (temp >= 9) {
        return "트렌치 코트, 야상, 점퍼, 스타킹, 기모바지";
      }
      if (temp >= 5) {
        return "울 코트, 히트텍, 가죽 옷, 기모";
      }
      if (temp < 5) {
        return "패딩, 두꺼운 코트, 누빔 옷, 기모, 목도리";
      }
      return "로딩중";
    }
  },
  methods: {
    // 계산된 getter
    fetchWeather: function (lat, lon) {
      const that = this;
      fetch(`http://api.openweathermap.org/data/2.5/weather?appid=0f4f911a6ae37361da7bfcf43622bb61&lang=KR&units=metric&lat=${lat}&lon=${lon}`)
        .then(res => {
          console.log({res})
          return res.json();
        })
        .then(data => {
          console.log({data}, that)
          that.cityName = data.name;
          that.temperature = data.main.temp;
        });
    }
  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="scss">
h3 {
  margin: 40px 0 0;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
</style>
