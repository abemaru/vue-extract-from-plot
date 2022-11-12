<template>
  <main>
		<div ref="plot"></div>
  </main>
</template>


<script lang="ts">
	import { defineComponent, onMounted, reactive, ref, watch } from "vue";
	import { Plotly } from "plotly.js-dist-min";

	export default defineComponent({
		name: "App",
		setup() {
			const plot = ref<HTMLDivElement>();

			const data = reactive([
				{
					x: [1,2,3,4,5],
					y: [10, 20, 30, 40, 50],
				},
			]);

			onMounted(() => {
				if (!plot.value) return;
				Plotly.newPlot(plot.value, data);
			});

			setInterval(() => {
				data[0].y[3] += 1;
				for (let i = 0; i < data[0].y.length; i++) {
					data[0].y[i] = Math.random();
				}
			}, 2000);

			watch(data, () => {
				if (!plot.value) return;
				Plotly.redraw(plot.value);
			});

			return { plot };
		},
	})

	
</script>
