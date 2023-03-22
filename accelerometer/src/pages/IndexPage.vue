<template>
  <q-page class="flex flex-center page">
    <h4 class="flex-center justify-evenly">Acelerómetro Beta</h4>
    <div class="full-width">
      <div class="acel-row col-12 row no-wrap justify-start items-center content-center">
        <label for="acel_x" class="acel-label col-1">X: </label>
        <div id="acel_x" class="col-11">{{ acel_x }}</div>
      </div>
      <div class="acel-row col-12 row no-wrap justify-start items-center content-center">
        <label for="acel_z" class="acel-label col-1">Y: </label>
        <div id="acel_y" class="col-11">{{ acel_y }}</div>
      </div>
      <div class="acel-row col-12 row no-wrap justify-start items-center content-center">
        <label for="acel_y" class="acel-label col-1">Z: </label>
        <div id="acel_z" class="col-11">{{ acel_z}}</div>
      </div>
    </div>
    <div class="btn-group fit row wrap justify-evenly items-center content-center">
      <q-btn color="primary" label="Iniciar" class="col-auto self-center" @click="startAcel" />
      <q-btn color="primary" label="Parar" class="col-auto self-center" @click="stopAcel" />
    </div>
  </q-page>
</template>

<script >
import { defineComponent } from "vue";
import { Motion } from "@capacitor/motion";

export default defineComponent({
  name: "IndexPage",

data()
  {  
      let acel_x = 0;
      let acel_y = 0;
      let acel_z = 0;
    return {acel_x, acel_y, acel_z}
  },  
  methods: {
    async startAcel() {
      console.log("Device motion event: Start listening");
      // try {
      //   await DeviceMotionEvent.requestPermission();
      // } catch (e) {
      //   // Handle error
      //   return;
      // }
      await Motion.addListener("accel", (event) => {
        this.acel_x = event.acceleration.x;
        this.acel_y = event.acceleration.y;
        this.acel_z = event.acceleration.z;
      });
    },
    async stopAcel() {
      console.log("Device motion event: Stop listening");
      try {
          await Motion.removeAllListeners();
      } catch (e) {
        // Handle error
        console.error('error: ', e)
        return;
      }
    },
  },
});
</script>
<style lang="scss" scoped>
.page {
  display: flex;
  flex-direction: column;
  flex-wrap: nowrap;
  justify-content: flex-start;

  .acel-row {
    margin: 0 0.5em;
    font-size: 14pt;
    .acel-label {
      font-weight: bold;
    }
  }
    .btn-group {
    }
  button {
    margin: 0.25em;
  }
}
</style>
