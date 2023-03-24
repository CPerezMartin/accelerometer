<template>
  <q-page class="flex flex-center page">
    <h4 class="flex-center justify-evenly">Acelerómetro Beta</h4>
    <div class="visor_group full-width">
      <p
        class="visor_title col-12 flex justify-evenly items-center content-center"
      >
        Visor
      </p>
      <div
        class="visor_row col-12 row no-wrap justify-start items-center content-center"
      >
        <label for="acel_x" class="acel_label col-1">X: </label>
        <div id="acel_x" class="col-11">{{ acel_x }}</div>
      </div>
      <div
        class="visor_row col-12 row no-wrap justify-start items-center content-center"
      >
        <label for="acel_z" class="acel_label col-1">Y: </label>
        <div id="acel_y" class="col-11">{{ acel_y }}</div>
      </div>
      <div
        class="visor_row col-12 row no-wrap justify-start items-center content-center"
      >
        <label for="acel_y" class="acel_label col-1">Z: </label>
        <div id="acel_z" class="col-11">{{ acel_z }}</div>
      </div>
    </div>
    <div class="input_group col-12 row flex justify-evenly">
      <p
        class="input_title col-12 flex justify-evenly items-center content-center"
      >
        Cuota mínima de registro
      </p>
      <div class="input_item col-4 row no-wrap justify-evenly full-width">
        <q-input
          filled
          v-model="min"
          label="Valor de activación"
          id="input_x"
          type="number"
          step="0.5"
          class="col-3"
        />
        <!-- <q-input
          filled
          v-model="range_y"
          label="Eje Y"
          id="input_y"
          type="number"
          step="0.5"
          class="col-3"
        />
        <q-input
          filled
          v-model="range_z"
          label="Eje Z"
          id="input_z"
          type="number"
          step="0.5"
          class="col-3"
        /> -->
      </div>
    </div>
    <div
      class="btn_group fit row wrap justify-evenly items-center content-center"
    >
      <q-btn
        color="primary"
        label="Iniciar"
        class="col-auto self-center"
        @click="startAcel"
      />
      <q-btn
        color="primary"
        label="Parar"
        class="col-auto self-center"
        @click="stopAcel"
      />
      <q-btn
        color="primary"
        icon-right="archive"
        label="Exportar"
        no-caps
        @click="exportTable"
      ></q-btn>
    </div>
    <q-table
      class="acel_table full-width"
      title="Lecturas Aceleracion"
      :rows="accelRecords"
      :columns="columns"
      no-data-label="No hay datos disponibles"
      row-key="name"
      flat
      bordered
    />
  </q-page>
</template>

<script>
import { defineComponent, ref } from "vue";
import { Motion } from "@capacitor/motion";

export default defineComponent({
  name: "IndexPage",

  data() {
    let acel_x = 0;
    let acel_y = 0;
    let acel_z = 0;
    let min = 0;

    /** array to save acceleration data with timestamp. Format:: timestamp, x, y, z */
    let accelRecords = [];
    // let accelRecords = [
    //   {
    //     timestamp: 13,
    //     acel_x: 10,
    //     acel_5: 2,
    //     acel_z: 3,
    //   },
    //   {
    //     timestamp: 124,
    //     acel_x: 11,
    //     acel_5: 2,
    //     acel_z: 3,
    //   },
    //   {
    //     timestamp: 125,
    //     acel_x: 12,
    //     acel_5: 2,
    //     acel_z: 3,
    //   },
    //   {
    //     timestamp: 126,
    //     acel_x: 14,
    //     acel_5: 2,
    //     acel_z: 3,
    //   },
    //   {
    //     timestamp: 121,
    //     acel_x: 13,
    //     acel_5: 2,
    //     acel_z: 3,
    //   },
    //   {
    //     timestamp: 122,
    //     acel_x: 15,
    //     acel_5: 2,
    //     acel_z: 3,
    //   },
    //   {
    //     timestamp: 120,
    //     acel_x: 16,
    //     acel_5: 2,
    //     acel_z: 3,
    //   },
    //   {
    //     timestamp: 123,
    //     acel_x: 17,
    //     acel_5: 2,
    //     acel_z: 3,
    //   },
    //   {
    //     timestamp: 127,
    //     acel_x: 18,
    //     acel_5: 2,
    //     acel_z: 3,
    //   },
    //   {
    //     timestamp: 125,
    //     acel_x: 19,
    //     acel_5: 2,
    //     acel_z: 3,
    //   },
    //   {
    //     timestamp: 124,
    //     acel_x: 1,
    //     acel_5: 20,
    //     acel_z: 3,
    //   },
    //   {
    //     timestamp: 129,
    //     acel_x: 1,
    //     acel_5: 21,
    //     acel_z: 3,
    //   },
    // ];

    const columns = [
      { name: "x_Axys", label: "Eje X", field: "acel_x", align: "right" },
      { name: "y_Axys", label: "Eje Y", field: "acel_y", align: "right" },
      { name: "z_Axys", label: "Eje Z", field: "acel_z", align: "right" },
      {
        name: "timestamp",
        label: "Hora de registro",
        field: "timestamp",
        align: "left",
      },
    ];
    return {
      acel_x,
      acel_y,
      acel_z,
      min,

      accelRecords,
      columns,
    };
  },
  computed: {
    normalizeMin() {
      return Math.abs(this.min);
    },
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

        if (
          this.normalizeMin <= Math.abs(this.acel_x) ||
          this.normalizeMin <= Math.abs(this.acel_y) ||
          this.normalizeMin <= Math.abs(this.acel_z)
        ) {
          let timestamp = new Date().toISOString();
          this.accelRecords.push({
            timestamp: timestamp,
            acel_x: this.acel_x,
            acel_y: this.acel_y,
            acel_z: this.acel_z,
          });
        }
      });
    },
    async stopAcel() {
      console.log("Device motion event: Stop listening");
      try {
        await Motion.removeAllListeners();
      } catch (e) {
        // Handle error
        console.error("error: ", e);
        return;
      }
    },
    wrapCsvValue(val, formatFn, row) {
      let formatted = formatFn !== void 0 ? formatFn(val, row) : val;

      formatted =
        formatted === void 0 || formatted === null ? "" : String(formatted);

      formatted = formatted.split('"').join('""');
      /**
       * Excel accepts \n and \r in strings, but some other CSV parsers do not
       * Uncomment the next two lines to escape new lines
       */
      // .split('\n').join('\\n')
      // .split('\r').join('\\r')

      return `"${formatted}"`;
    },
    exportTable() {
      // naive encoding to csv format
      console.log("Testing: ", -this.min);
      const content = [this.columns.map((col) => wrapCsvValue(col.label))]
        .concat(
          rows.map((row) =>
            columns
              .map((col) =>
                wrapCsvValue(
                  typeof col.field === "function"
                    ? col.field(row)
                    : row[col.field === void 0 ? col.name : col.field],
                  col.format,
                  row
                )
              )
              .join(",")
          )
        )
        .join("\r\n");

      const status = exportFile("table-export.csv", content, "text/csv");

      if (status !== true) {
        $q.notify({
          message: "Browser denied file download...",
          color: "negative",
          icon: "warning",
        });
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

  .visor_group {
    background-color: aliceblue;
    padding-bottom: 1em;
    border-radius: 5px;
    margin-bottom: 1em;

    .visor_title {
      font-size: x-large;
      font-weight: bolder;
    }

    .visor_row {
      margin: 0 0.5em;
      font-size: 14pt;
      .acel_label {
        font-weight: bold;
      }
    }
  }
  .input_group {
    margin-bottom: 1em;

    .input_title {
      font-size: larger;
      font-weight: bold;
    }
  }
  .btn_group {
  }
  button {
    margin: 0.25em;
  }

  .acel_table {
    margin: 1em 0;

    tbody {
      tr:nth-child(5n) {
        background-color: lightblue;
      }
    }

    td:first-child {
      background-color: lightblue;
    }
  }
}
</style>
