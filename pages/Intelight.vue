<template>
  <div class="to-do-container">
    <div class="text-center">
      <logo />
      <vuetify-logo />
    </div>
    <div class="create-container">
      <div id="video_box">
        <div id="lightOne" class="one staticOne" @click="toggleLight(1)" @mouseover="onMouse" @mouseleave="offMouse">
          <h3>Lamp 01</h3>
        </div>
        <div id="lightTwo" class="two staticTwo" @click="toggleLight(2)" @mouseover="onMouse2" @mouseleave="offMouse2">
          <h3>Lamp 2</h3>
        </div>
        <div id="lightThree" class="three staticThree" @click="toggleLight(3)" @mouseover="onMouse3" @mouseleave="offMouse3">
          <h3>Lamp3</h3>
        </div>
        <div class="text-center">
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
    </div>
    <v-card flat color="transparent">
      <v-subheader>
        <h3>
          Luces
          Regulables
        </h3>
      </v-subheader>
      <v-card-text class="pt-3">
        <h4>
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
        </h4>
      </v-card-text>
    </v-card>
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

    onMouse () {
      document.getElementById('lightOne').classList.add('one')
    },
    offMouse () {
      document.getElementById('lightOne').classList.remove('one')
    },

    onMouse2 () {
      document.getElementById('lightTwo').classList.add('two')
    },
    offMouse2 () {
      document.getElementById('lightTwo').classList.remove('two')
    },
    onMouse3 () {
      document.getElementById('lightThree').classList.add('three')
    },
    offMouse3 () {
      document.getElementById('lightThree').classList.remove('three')
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
    border-style: dashed;
  border-width: thin;
  border-color: rgba(0, 255, 64, 0.87);
}
.one {
  border-style: dashed;
  border-width: thin;
  border-color: rgba(0, 255, 64, 0.87);
  background-color: #0000ff61;

}

.staticOne {
  position:absolute;
  float:left;
  width:705px;
  min-height:80px;
  z-index:300000;
  top: 277px;
  left: 462px;
}
.two {
  border-style: dashed;
  border-width: thin;
  border-color: rgba(0, 255, 136, 0.637);
  background-color: #0000ff61;

}

.staticTwo {
  position:absolute;
  float:left;
  width:705px;
  min-height:80px;
  z-index:300000;
  top: 158px;
  left: 462px;
}
.three {
  border-style: dashed;
  border-width: thin;
  border-color: rgba(0, 255, 221, 0.808);
  background-color: #0000ff61;
  }

.staticThree {
  position:absolute;
  float:left;
  width:40px;
  min-height:40px;
  z-index:300000;
  top: 440px;
  left: 750px;
}
</style>
