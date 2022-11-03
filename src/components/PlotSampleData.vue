<template>
  <div ref="plot"></div>
</template>

<script lang="ts">
import { defineComponent, onMounted, reactive, ref, watch } from "vue";
import { Plotly } from "plotly.js-dist-min";

export default defineComponent({
  setup() {
    const plot = ref<HTMLDivElement>();
    const sampleData = reactive([
      {
        Month: [
          "1-01",
          "1-02",
          "1-03",
          "1-04",
          "1-05",
          "1-06",
          "1-07",
          "1-08",
          "1-09",
        ],
        Sales: [266.0, 145.9, 183.1, 119.3, 180.3, 168.5, 231.8, 224.5, 192.8],
      },
    ]);

    onMounted(() => {
      if (!plot.value) return;
      Plotly.newPlot(plot.value, sampleData);
    });

    setInterval(() => {
      sampleData[0].Sales[3] += 1;
      for (let i = 0; i < sampleData[0].Sales.length; i++) {
        sampleData[0].Sales[i] = Math.random();
      }
    }, 2000);

    watch(sampleData, () => {
      if (!plot.value) return;
      Plotly.redraw(plot.value);
    });

    return { plot };
  },
});
</script>
