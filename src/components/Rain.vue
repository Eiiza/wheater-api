<template>
  <svg
    class="cloud"
    version="1.1"
    xmlns="http://www.w3.org/2000/svg"
    xmlns:xlink="http://www.w3.org/1999/xlink"
    x="0px"
    y="0px"
    viewBox="0 0 100 125"
    enable-background="new 0 0 100 100"
    xml:space="preserve"
  >
    <path
      fill="#ccc"
      d="M81.82,71.144v-9.958c5.425-3.149,9.088-9.003,9.088-15.729c0-10.043-8.138-18.185-18.18-18.185
    c-3.643,0-7.011,1.093-9.852,2.935C60.075,18.123,49.3,9.089,36.363,9.089C21.3,9.089,9.092,21.3,9.092,36.363
    c0,8.068,3.521,15.295,9.087,20.29v11.169C7.322,61.529,0,49.816,0,36.363C0,16.279,16.279,0,36.363,0
    c13.648,0,25.523,7.528,31.743,18.646c1.51-0.257,3.037-0.466,4.622-0.466C87.791,18.179,100,30.392,100,45.457
    C100,57.323,92.404,67.396,81.82,71.144z"
    />
    <path
      fill="#4dacff"
      class="rainny"
      d="M31.817,45.457c2.509,0,4.546,2.036,4.546,4.546v9.087c0,2.513-2.037,4.546-4.546,4.546
    c-2.512,0-4.546-2.033-4.546-4.546v-9.087C27.271,47.49,29.306,45.457,31.817,45.457z"
    />
    <path
      fill="#4dacff"
      class="rainny"
      d="M31.817,72.729c2.509,0,4.546,2.035,4.546,4.546v9.087c0,2.513-2.037,4.547-4.546,4.547
    c-2.512,0-4.546-2.034-4.546-4.547v-9.087C27.271,74.762,29.306,72.729,31.817,72.729z"
    />
    <path
      fill="#4dacff"
      class="rainny rainny2"
      d="M49.997,54.544c2.512,0,4.547,2.038,4.547,4.546v9.093c0,2.51-2.035,4.546-4.547,4.546
    c-2.509,0-4.542-2.036-4.542-4.546V59.09C45.456,56.582,47.49,54.544,49.997,54.544z"
    />
    <path
      fill="#4dacff"
      class="rainny rainny2"
      d="M49.997,81.82c2.512,0,4.547,2.035,4.547,4.541v9.093c0,2.513-2.035,4.546-4.547,4.546
    c-2.509,0-4.542-2.033-4.542-4.546v-9.093C45.456,83.854,47.49,81.82,49.997,81.82z"
    />
    <path
      fill="#4dacff"
      class="rainny rainny3"
      d="M68.183,45.457c2.508,0,4.546,2.036,4.546,4.546v9.087c0,2.513-2.038,4.546-4.546,4.546
    c-2.514,0-4.547-2.033-4.547-4.546v-9.087C63.636,47.49,65.669,45.457,68.183,45.457z"
    />
    <path
      fill="#4dacff"
      class="rainny rainny3"
      d="M68.183,72.729c2.508,0,4.546,2.035,4.546,4.546v9.087c0,2.513-2.038,4.547-4.546,4.547
    c-2.514,0-4.547-2.034-4.547-4.547v-9.087C63.636,74.762,65.669,72.729,68.183,72.729z"
    />
  </svg>
</template>

<style scoped>
@keyframes cloud_drift {
  0% {
    transform: translateY(5px);
  }
  50% {
    transform: translateY(-5px);
  }
  100% {
    transform: translateY(5px);
  }
}

@keyframes rain_drop {
  0% {
    transform: translateY(0px);
  }
  100% {
    transform: translateY(50px);
    opacity: 0;
  }
}

svg {
  animation: cloud_drift 4s infinite linear;
  filter: drop-shadow(10px 10px 10px rgba(0, 0, 0, 0.4));
}

.rainny {
  fill: rgba(#4dacff, 0.8);
  animation: rain_drop 1s infinite linear;
}
.rainny2 {
  animation: rain_drop 1s infinite linear;
  animation-delay: -0.4s;
}
.rainny3 {
  animation: rain_drop 1s infinite linear;
  animation-delay: -0.7s;
}
</style>
