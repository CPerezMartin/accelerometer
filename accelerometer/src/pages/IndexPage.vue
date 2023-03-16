<template>
  <q-page class="flex flex-center page">
    <h3 class="flex-center">Accelerometer Testing</h3>
    <div>
      <label for="acel_x">Aceleración X: </label>
      <p id="acel_x">{{ acel_x }}</p>
      <label for="acel_z">Aceleración Z: </label>
      <p id="acel_y">{{ acel_y }}</p>
      <label for="acel_y">Aceleración Y: </label>
      <p id="acel_z">{{ acel_z}}</p>
    </div>
    <div flex flex-center>
      <q-btn color="primary" label="Iniciar Acelerometro" @click="startAcel" />
      <q-btn color="primary" label="Parar Acelerometro" @click="stopAcel" />
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
      let acel_x =0;
      let acel_y =0;
      let acel_z =0;
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
        console.log("🚀 ~ file: IndexPage.vue:54 ~ Motion.addListener ~ event: ", event.acceleration.x)
        this.acel_x = event.acceleration.x;
        this.acel_y = event.acceleration.y;
        this.acel_z = event.acceleration.z;
      });
    },
    stopAcel() {
      console.log("Device motion event: Stop listening");
      try {
          Motion.removeAllListeners('accel');
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
}
</style>
