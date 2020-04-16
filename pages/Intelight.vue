<template>
  <div class="to-do-container">
    <div class="text-center">
      <logo />
      <vuetify-logo />
    </div>
    <div class="create-container">
      <!-- <button @click="create">
        Add
      </button> -->
      <v-text-field color="success" loading disabled />
      <div id="video_box">
        <div id="lightOne" @click="toggleLight(1)">
          1
        </div>
        <div id="lightTwo" @click="toggleLight(2)">
          2
        </div>
        <div id="lightThree" @click="toggleLight(3)">
          3
        </div>
        <div id="video">
          <img
            style="-webkit-user-select: none;margin: auto;"
            src="http://dawn.ii.uam.es/mjpg/video.mjpg"
            width="auto"
            height="auto"
          >
        </div>
      </div>
    </div>
    <v-card flat color="transparent">
      <v-subheader>
        Luces
        Regulables
      </v-subheader>

      <v-card-text class="pt-3">
        <v-slider
          v-model="value"
          max="50"
          min="0"
          :rules="rules"
          label="Intensidad?"
          step="01"
          thumb-label="always"
          ticks
          @change="onChange"
        />
      </v-card-text>
    </v-card>
    <b />
    <v-text-field color="success" loading disabled />
    <div class="to-do-list-container" />
  </div>
</template>

<script>
import axios from 'axios'
import Logo from '~/components/Logo.vue'
import VuetifyLogo from '~/components/VuetifyLogo.vue'

export default {
  components: {
    Logo,
    VuetifyLogo
  },
  props: {
    todos: {
      type: Array,
      default () {
        return []
      }
    }
  },
  asyncData ({ light, value }) {

  },
  data () {
    return {

      light: 0,
      value: 25,
      rules: [
        v => v <= 30 || 'Intensidad máx. 50'
      ]
    }
  },
  created () {
    setInterval(() => {
      axios.get('https://us-central1-touchlightbr.cloudfunctions.net/app/api/v1/light/4/status')
        .then((res) => {
          this.value = res.data.status
          console.log(this.value)
        })
    }, 3000)

    axios.get('https://us-central1-touchlightbr.cloudfunctions.net/app/api/v1/light/4/status')
      .then((res) => {
        this.value = res.data.status
      })
  },
  methods: {
    onChange () {
      axios.post(`https://us-central1-touchlightbr.cloudfunctions.net/app/api/v1/light/4/value/${this.value}`)
        .then((res) => {
          // console.log(res)

        })
        .catch((err) => {
          console.log(err)
        })
    },

    toggleLight (light) {
      axios.get(`https://us-central1-touchlightbr.cloudfunctions.net/app/api/v1/light/${light}/status`)
        .then((res) => {
          console.log('passo 1', res.data.status)
          let status = 0
          if (res.data.status === '0') {
            status = 1
          }
          return status
        })
        .then((status) => {
          console.log('passo 2', status)
          axios.post(`https://us-central1-touchlightbr.cloudfunctions.net/app/api/v1/light/${light}/value/${status}`)
            .then((res) => {
              // console.log(res)

            })
            .catch((err) => {
              console.log(err)
            })
        })
        .catch((err) => {
          console.log(err)
        })
    }
  }
}
</script>

<style lang="scss">
#video {
  margin-top: 2em;
}

#video_box{
    float:center;
}
#lightOne {
  border-style: dashed;
  border-width: thin;
  border-color: red;
  position:absolute;
  float:left;
  width:40px;
  min-height:40px;
  background-color: #0000ff61;
  z-index:300000;
  top: 308px;
  left: 100px;
}

#lightTwo {
  border-style: dashed;
  border-width: thin;
  border-color: red;
  position:absolute;
  float:left;
  width:40px;
  min-height:40px;
  background-color: #0000ff61;
  z-index:300000;
  top: 308px;
  left: 240px;
}

#lightThree {
  border-style: dashed;
  border-width: thin;
  border-color: red;
  position:absolute;
  float:left;
  width:40px;
  min-height:40px;
  background-color: #0000ff61;
  z-index:300000;
  top: 308px;
  left: 160px;
}
</style>
