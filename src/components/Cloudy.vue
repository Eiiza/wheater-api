<template>
  <svg
    class="small_cloud"
    version="1.1"
    xmlns="http://www.w3.org/2000/svg"
    xmlns:xlink="http://www.w3.org/1999/xlink"
    x="0px"
    y="0px"
    viewBox="-10 -20 130 100"
    enable-background="new 0 0 100 72.726"
    xml:space="preserve"
  >
    <path
      fill="#ccc"
      d="M36.363,72.726C16.281,72.726,0,56.447,0,36.364C0,16.279,16.281,0,36.363,0
	c13.648,0,25.523,7.528,31.743,18.646c1.51-0.257,3.041-0.466,4.622-0.466C87.791,18.18,100,30.393,100,45.455
	c0,15.059-12.209,27.271-27.271,27.271C66.42,72.726,43.633,72.726,36.363,72.726z M72.729,63.634
	c10.042,0,18.184-8.143,18.184-18.18c0-10.041-8.142-18.183-18.184-18.183c-3.639,0-7.006,1.093-9.852,2.935
	C60.08,18.123,49.3,9.09,36.363,9.09C21.303,9.09,9.092,21.3,9.092,36.364c0,15.062,12.211,27.271,27.271,27.271
	C44.585,63.634,65.864,63.634,72.729,63.634z"
    />
  </svg>
</template>
<style lang="sass" scoped>
@keyframes cloud_drift
  0%
    transform: translateY(5px)
  50%
    transform: translateY(-5px)
  100%
    transform: translateY(5px)
      
svg
  animation: cloud_drift 4s infinite ease-in
</style>