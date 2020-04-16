<template>
  <img
    class="VuetifyLogo"
    alt="Vuetify Logo"
    src="https://pbs.twimg.com/profile_images/1164841483079282694/dvG65MeJ_400x400.png"
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
