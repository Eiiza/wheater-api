<template>
  <div
    v-if="temperatureArr && weatherArr && weatherArr.length"
    class="daybox"
    @click="toggleBoxClick"
  >
    <div class="top" :class="[isMorning ? 'is_morning' : 'is_night']">
      <div class="change">
        <div class="moon"></div>
        <svg class="galaxy12">
          <circle cx="25" cy="130" r="0.6"></circle>
          <circle cx="140" cy="70" r="1"></circle>
          <circle cx="320" cy="80" r="0.8"></circle>
          <circle cx="360" cy="180" r="1"></circle>
          <circle cx="300" cy="200" r="1"></circle>
          <circle cx="220" cy="140" r="0.8"></circle>
          <circle cx="180" cy="160" r="1.6"></circle>
          <circle cx="100" cy="200" r="0.8"></circle>
          <circle cx="250" cy="220" r="1"></circle>
          <circle cx="140" cy="220" r="0.8"></circle>
          <circle cx="80" cy="230" r="1.2"></circle>
          <circle cx="160" cy="250" r="1.2"></circle>
        </svg>
        <component :is="changeToWeather(weatherArr[0])" class="weather" />
      </div>
      <svg
        v-if="weekTemperatureArr && weekTemperatureArr.length"
        class="temp_svg"
      >
        <circle
          v-for="(item, index) in weekTemperatureArr"
          :key="`point-${index}`"
          class="temp_svg-point"
          :cx="item.x"
          :cy="item.y"
          r="3"
        />
        <text
          v-for="(item, index) in weekTemperatureArr"
          :key="`text-${index}`"
          class="temp_svg-temperature_num"
          :x="item.x"
          :y="item.y"
        >
          {{ `${item && item.temperature}℃` }}
        </text>
        <polyline v-if="lineCoo" class="temp_svg-line" :points="lineCoo" />
      </svg>
      <div class="text_area">
        <div
          v-show="weekTemperatureArr && weekTemperatureArr.length"
          class="text_area-temp"
        >
          {{ `${runningTempNum || temperatureArr.morning[0]}℃` }}
        </div>
        <div class="text_area-info">
          <div>Taipei City</div>
          <div>{{ `${today} ${nowTime}` }}</div>
        </div>
      </div>
    </div>
    <div class="bottom" v-if="weatherArr && weatherArr.length">
      <div class="daywt" v-for="(item, index) in weatherArr" :key="index">
        <h3>{{ getWeek(index) }}</h3>
        <component :is="changeToWeather(item)" class="weather" />
      </div>
    </div>
  </div>
</template>

<script>
import moment from "moment";
import Rain from "./Rain";
import Sun from "./Sun";
import CloudySun from "./CloudySun";
import Cloudy from "./Cloudy";
import NP from "number-precision";

export default {
  name: "WeatherBox",
  props: {
    temperatureArr: {
      type: Object,
      default: () => {},
    },
    weatherArr: {
      type: Array,
      default: () => [],
    },
  },
  components: {
    Rain,
    Sun,
    CloudySun,
    Cloudy,
  },
  data() {
    return {
      today: moment(new Date().getTime()).format("dddd"),
      nowTime: undefined,
      isMorning: true,
      isRunAni: false,
      runningTempNum: 0,
    };
  },
  computed: {
    weekTemperatureArr() {
      const vm = this;
      const result = [];
      const type = vm.isMorning ? "morning" : "night";
      if (vm.temperatureArr && type) {
        vm.temperatureArr[type].forEach((e, index) => {
          if (index > 6) return;
          result.push({
            x: vm.svgCalculateX(index),
            y: vm.svgCalculateY(e),
            temperature: e,
          });
        });
      }

      return result || [];
    },
    lineCoo() {
      const vm = this;
      let result = "";
      const type = vm.isMorning ? "morning" : "night";

      if (vm.temperatureArr && type) {
        vm.temperatureArr[type].forEach((e, index) => {
          if (index > 6) return;
          result = result.concat(
            `${vm.svgCalculateX(index)},${vm.svgCalculateY(e)} `
          );
        });
      }
      return result;
    },
  },
  mounted() {
    const vm = this;
    setInterval(() => {
      vm.nowTime = moment(new Date().getTime()).format("H:mm:ss");
    }, 1000);
  },
  methods: {
    getWeek(addDate = 0) {
      return moment(new Date().getTime())
        .add(addDate, "days")
        .format("dddd")
        .slice(0, 3)
        .toUpperCase();
    },
    changeToWeather(val) {
      switch (Math.floor(val / 3)) {
        case 0:
          return "Sun";
        case 1:
          return "CloudySun";
        case 2:
          return "Cloudy";
        default:
          return "Rain";
      }
    },
    animateValue(start = 0, end = 100, precision = 0) {
      const vm = this;
      try {
        vm.isRunAni = true;
        let startTimestamp = null;
        const duration = 500;

        const step = (timestamp) => {
          if (!startTimestamp) startTimestamp = timestamp;
          const progress = Math.min(
            NP.divide(NP.minus(+timestamp, +startTimestamp), +duration),
            1
          );

          vm.runningTempNum = NP.plus(
            NP.times(+progress, NP.minus(+end, +start)).toFixed(precision),
            +start
          );
          if (progress < 1) {
            window.requestAnimationFrame(step);
          }
        };
        window.requestAnimationFrame(step);
        setTimeout(() => {
          vm.isRunAni = false;
        }, 200);
      } catch (error) {
        console.log(error);
      }
    },
    toggleBoxClick() {
      const vm = this;
      const todayMorTemp = vm.temperatureArr.morning[0];
      const todayNigTemp = vm.temperatureArr.night[0];
      vm.isMorning = !vm.isMorning;
      vm.animateValue(
        vm.isMorning ? todayNigTemp : todayMorTemp,
        vm.isMorning ? todayMorTemp : todayNigTemp,
        0
      );
    },
    svgCalculateX(i) {
      return 50 * (i + 1);
    },
    svgCalculateY(e) {
      return 320 - e * 10;
    },
  },
};
</script>

<style lang="sass" scoped>
@mixin size($w,$h)
  width: $w
  height: $h
$cwhite: #f3f3f3
$cyellow: #ffd633
$cblack: #222
$cblue_start: #1a0e36
$cblue_end: #1c5473

.daybox
  width: 400px
  background-color: $cwhite
  box-shadow: 0px 0px 20px rgba(black,0.5)
  border-radius: 10px //因為上面有一個div所以上面不會變
  cursor: pointer

  &:hover
    & .bottom
      height: 100px
    & .top
      .temp_svg
        opacity: 1
        &-temperature_num
          opacity: 1
    & .daywt
      height: 100px
      opacity: 1
      top: 0px

.temp_svg
  width: 100%
  opacity: 0.2
  &-line
    stroke: $cwhite
    stroke-width: 1px
    fill: none
  &-temperature_num
    transform: translateX(-5px) translateY(17px)
    font-size: 10px
    fill: $cwhite
    opacity: 0
  &-point
    fill: $cwhite

.top
  height: 330px
  transition: 3s
  background: linear-gradient(20deg,$cblue_start,$cblue_end) //線性漸層&角度傾斜
  background-size: 100% 300% //(w,h)把背景用很長然後移動它，看起來就有變深的感覺
  border-radius: 7px 7px 0px 0px
  overflow: hidden
  background-position: 100% 30%

  .galaxy12
    position: absolute
    +size(100%,280px)
    top: 200px
    circle
    fill: none
    stroke: $cwhite
    animation: sparkle 2s infinite ease-in-out

  .change
    position: absolute
    +size(150%,150%)
    top: 50%
    left: -20%
    transform: rotate(0deg)
    transition-duration: 3s

  .weather
    +size(120px,120px)
    position: absolute
    left: 320px
    bottom: 350px

  &.is_night
    background-position: 100% 100%

    .change
      transform: rotate(180deg)

  .moon
    +size(70px,70px)
    position: absolute
    left: 180px
    top: 370px
    border-radius: 100%
    box-shadow: 15px -15px
    transition: 0.5s
    animation: moon_move 15s both

.bottom
  display: flex
  justify-content: center
  box-shadow: 0px 0px 20px rgba(black,0.5)
  border-radius: 0px 0px 7px 7px
  //要做動畫的部分
  height: 10px
  overflow: hidden
  transition-duration: 1s

  .daywt
    width: calc(100% / 7 - 5px)
    display: inline-flex
    align-items: center
    flex-direction: column
    text-align: center
    justify-content: center
    opacity: 0
    transition: 0.5s
    height: 100px

    h3
      font-size: 8px
      color: rgba(black,0.5)
      font-weight: 400
    .weather
      +size(40px,40px)

  @for $i from 1 through 7

    .daywt:nth-child(#{$i})
      transition-delay: 0.1s * $i

.text_area
  display: inline-block
  position: absolute
  bottom: 25px
  left: 25px
  color: $cwhite

  &-temp
    transition: 3s
    font-size: 75px
    line-height: 75px
  &-info
    font-size: 12px
    opacity: 0.5

    >div
      padding: 2px 0

.temp_svg
  circle
  transition: 0.3s
  polyline
  transition: 0.3s 0.3s
  text
  transition: 0.3s 0.6s

@keyframes moon_move
  0%
    box-shadow: 15px -15px $cyellow
    transform: translateX(-15px) translateY(15px)
  100%
    box-shadow: -30px 30px $cyellow
    transform: translateX(30px) translateY(-30px)

@keyframes daylight
  0%
    background-position: 100% 30%
  100%
    background-position: 100% 100%

@keyframes sparkle
  0%
    opacity: 1
  50%
    opacity: 0
  100%
    opacity: 1
</style>
