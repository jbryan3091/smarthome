<template>
  <img
    class="VuetifyLogo"
    alt="Vuetify Logo"
    src="https://pbs.twimg.com/profile_images/1098140636295905280/E1YRufoX_400x400.png"
  >
</template>

<style>
.VuetifyLogo {
  height:180px;
  width: 180px;
  transform: rotateY(560deg);
  animation: turn 3.5s ease-out forwards 1s;
}

@keyframes turn {
  100% {
    transform: rotateY(0deg);
  }
}
</style>
