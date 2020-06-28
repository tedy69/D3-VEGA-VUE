<template>
  <div id="app">
    <h1>{{ msg }} <a href="https://github.com/timelyportfolio/vue-vega">Github</a> </h1>
    <div style="display:flex;">
      <div style="flex:1;">
        <h3>SVG <a href="https://github.com/kristw/react-vega/blob/master/demo/src/vega/spec1.js">(spec1)</a> with Signal Listener/Emitter</h3>
        <Vega :spec="object" @tooltip="logEvent"></Vega>
      </div>
      <div style="flex:1;">
        <h3>Canvas <a href="https://github.com/kristw/react-vega/blob/master/demo/src/vega/spec2.js">(spec2)</a></h3>
        <Vega :spec="object" renderer="canvas"></Vega>
      </div>
    </div>
  </div>
</template>

<script>
/* eslint-disable */
import Vega from '../components/Vega.vue';

import axios from 'axios'
export default {
  name: 'app',
  components: {
    Vega
    },
  data(){
    return{
      msg: 'Welcome to Vue.js + Vega',
      object: {}
    }
  },
  mounted() {
    this.load()
  },
  methods: {
    load(){
        axios.get('http://192.168.20.227:1135/vega/visualization/get-by-id?id=visualization%3A1bb56580-16a1-11ea-a855-978f47f42f8e&start=2020-06-18%7C00%3A00%3A00&end=2025-01-01%7C00%3A00%3A00')
        .then(res => {
        this.object = res.data //respon dari rest api dimasukan ke users
      }).catch ((err) => {
        console.log(err);
      })
    }
   }
  }

// import spec1 from '../specs/spec1.js';
// import spec2 from '../specs/spec2.js';

// let spec3 = JSON.parse(JSON.stringify(spec2));
// let spec4 = JSON.parse(JSON.stringify(spec2));

// // for debugging or updating
// window.spec1 = spec1;
// window.spec2 = spec2;
// window.spec3 = spec3;
// window.spec4 = spec4;

// export default {
//   name: 'app',
//   components: {
//     Vega
//   },
//   data () {
//     return {
//       msg: 'Welcome to Vue.js + Vega',
//       spec1: spec1,
//       spec2: spec2,
//       spec3: spec3,
//       spec4: spec4
//     }
//   },
//   methods: {
//     logEvent: function(name, value) {
//       console.log(name,value);
//     }
//   }
// }
</script>

