<template>
  <div ref="chartElement" class="spectrogram"></div>
</template>

<script setup>
import { ref, onMounted, onBeforeUnmount, watch } from 'vue'
import Plotly from 'plotly.js-dist-min'

const props = defineProps({
  // Time values, one per FFT frame
  times: {
    type: Array,
    default: () => []
  },

  // Frequency values, one per frequency bin
  frequencies: {
    type: Array,
    default: () => []
  },

  // Matrix: one row per frequency, one column per time
  magnitude: {
    type: Array,
    default: () => []
  }
})

const chartElement = ref(null)

let observer
let mounted = false

async function draw() {
  if (!mounted || !chartElement.value) return

  const valid =
    props.times.length > 0 &&
    props.frequencies.length > 0 &&
    props.magnitude.length === props.frequencies.length &&
    props.magnitude.every(
      row =>
        row.length === props.times.length &&
        row.every(Number.isFinite)
    )

  if (!valid) {
    await Plotly.purge(chartElement.value)
    return
  }

  await Plotly.react(
    chartElement.value,
    [
      {
        type: 'surface',
        x: props.times,
        y: props.frequencies,
        z: props.magnitude,
        colorscale: 'Viridis',
        colorbar: {
          title: { text: 'Magnitude' }
        },
        hovertemplate:
          'Time: %{x:.3f} s<br>' +
          'Frequency: %{y:.2f} Hz<br>' +
          'Magnitude: %{z:.4f}<extra></extra>'
      }
    ],
    {
      autosize: true,
      margin: { l: 0, r: 10, t: 10, b: 0 },
      paper_bgcolor: '#f5f4f4',
      scene: {
        xaxis: { title: 'Time (s)' },
        yaxis: { title: 'Frequency (Hz)' },
        zaxis: { title: 'Magnitude' },
        aspectmode: 'auto'
      }
    },
    {
      responsive: true,
      displaylogo: false
    }
  )
}

watch(
  () => [props.times, props.frequencies, props.magnitude],
  draw,
  { deep: true }
)

onMounted(() => {
  mounted = true
  draw()

  observer = new ResizeObserver(() => {
    if (chartElement.value) {
      Plotly.Plots.resize(chartElement.value)
    }
  })

  observer.observe(chartElement.value)
})

onBeforeUnmount(() => {
  mounted = false
  observer?.disconnect()

  if (chartElement.value) {
    Plotly.purge(chartElement.value)
  }
})
</script>

<style scoped>
.spectrogram {
  width: 100%;
  height: 100%;
  min-width: 0;
  min-height: 0;
}
</style>
