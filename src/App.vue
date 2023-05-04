<template>
  <div id="app">
    <WeatherBox :temperatureArr="temperatureArr" :weatherArr="weatherArr" />
  </div>
</template>

<script>
import WeatherBox from "./components/WeatherBox";

export default {
  name: "App",
  components: {
    WeatherBox,
  },
  data() {
    return {
      temperatureArr: { morning: [], night: [] },
      weatherArr: [],
      requestUrl:
        "https://opendata.cwb.gov.tw/api/v1/rest/datastore/F-D0047-063?Authorization=CWB-7FAEFAAD-1E41-4FAC-9594-E729DBE8FF23&locationName=%E5%85%A7%E6%B9%96%E5%8D%80&elementName=T,Wx",
    };
  },
  async mounted() {
    const vm = this;
    const { success, records } = await vm.postData(vm.requestUrl, undefined);

    if (success && success === "true") {
      // 天氣資料
      const weatherElement = records?.locations[0].location[0].weatherElement.filter(
        (e) => (e.elementName = "Wx")
      );
      Object.entries(weatherElement[0].time).forEach(([key, value]) => {
        Object.entries(value).forEach(([subKey, subValue]) => {
          if (subKey === "elementValue") {
            const time = value.endTime.endsWith("18:00:00")
              ? "morning"
              : "night";
            vm.temperatureArr[time].push(+subValue[0].value);
          }
        });
      });

      Object.entries(weatherElement[1].time).forEach(([key, value]) => {
        if (key % 2 !== 0) return;
        Object.entries(value).forEach(([subKey, subValue]) => {
          if (subKey === "elementValue") {
            vm.weatherArr.push(+subValue[1].value);
          }
        });
      });
    }
  },
  methods: {
    postData(url, data) {
      return fetch(url, {
        body: JSON.stringify(data), // must match 'Content-Type' header
        cache: "no-cache", // *default, no-cache, reload, force-cache, only-if-cached
        credentials: "same-origin", // include, same-origin, *omit
        headers: {
          "user-agent": "Mozilla/4.0 MDN Example",
          Accept: "application/json",
        },
        method: "GET", // *GET, POST, PUT, DELETE, etc.
        mode: "cors", // no-cors, cors, *same-origin
        redirect: "follow", // manual, *follow, error
        referrer: "no-referrer", // *client, no-referrer
      }).then((response) => response.json()); // 輸出成 json
    },
  },
};
</script>

<style lang="sass" scoped>
*
  position: relative
  font-family: 'Martel Sans', sans-serif,微軟正黑體

html, body, #app
  width: 100%
  height: 100%
  margin: 0
  padding: 0
  display: flex
  justify-content: center
  align-items: center
</style>
