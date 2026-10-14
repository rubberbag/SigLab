<template>
  <div class="summary">
    <div class="summary-title">
      Panel F [Time-Frequency Summary]
    </div>

    <div class="summary-grid">
      <div class="metric">
        <span class="label">Dominant Frequency</span>
        <strong>{{ dominantFrequency }}</strong>
      </div>

      <div class="metric">
        <span class="label">Peak Magnitude</span>
        <strong>{{ peakMagnitude }}</strong>
      </div>

      <div class="metric">
        <span class="label">Frequency Range</span>
        <strong>{{ frequencyRange }}</strong>
      </div>

      <div class="metric">
        <span class="label">Time Window</span>
        <strong>{{ timeWindow }}</strong>
      </div>
    </div>

    <div class="interpretation">
      <span class="label">Observation</span>
      <p>{{ observation }}</p>
    </div>
  </div>
</template>

<script setup>
import { computed } from 'vue'

const props = defineProps({
  times: { type: Array, default: () => [] },
  frequencies: { type: Array, default: () => [] },
  magnitude: { type: Array, default: () => [] }
})

const peak = computed(() => {
  let result = {
    frequency: null,
    time: null,
    magnitude: null
  }

  props.magnitude.forEach((row, frequencyIndex) => {
    row.forEach((value, timeIndex) => {
      if (!Number.isFinite(value)) return

      if (result.magnitude === null || value > result.magnitude) {
        result = {
          frequency: props.frequencies[frequencyIndex],
          time: props.times[timeIndex],
          magnitude: value
        }
      }
    })
  })

  return result
})

const dominantFrequency = computed(() =>
  peak.value.frequency === null
    ? '—'
    : `${peak.value.frequency.toFixed(2)} Hz`
)

const peakMagnitude = computed(() =>
  peak.value.magnitude === null
    ? '—'
    : peak.value.magnitude.toFixed(4)
)

const frequencyRange = computed(() => {
  if (!props.frequencies.length) return '—'

  return `${Math.min(...props.frequencies)} – ${
    Math.max(...props.frequencies)
  } Hz`
})

const timeWindow = computed(() => {
  if (!props.times.length) return '—'

  const start = props.times[0]
  const end = props.times[props.times.length - 1]

  return `${start.toFixed(2)} – ${end.toFixed(2)} s`
})

const observation = computed(() => {
  if (peak.value.frequency === null) {
    return 'Waiting for time-frequency data.'
  }

  return `The strongest observed component is ${
    peak.value.frequency.toFixed(2)
  } Hz at ${
    peak.value.time.toFixed(2)
  } s, with magnitude ${
    peak.value.magnitude.toFixed(4)
  }.`
})
</script>

<style scoped>
.summary {
  width: 100%;
  height: 100%;
  min-width: 0;
  overflow: auto;
  padding: 8px;
  box-sizing: border-box;
  border:1px solid;
  border-inline-start: 0;
}

.summary-title {
  font-size: 14px;
  font-weight: 600;
  margin-bottom: 14px;
}

.summary-grid {
  display: grid;
  grid-template-columns: repeat(2, minmax(0, 1fr));
  gap: 10px;
}

.metric {
  display: flex;
  flex-direction: column;
  gap: 8px;
  padding: 12px;
  min-width: 0;
}

.label {
  font-size: 12px;
}

.metric strong {
  font-size: 18px;
  overflow-wrap: anywhere;
}

.interpretation {
  margin-top: 12px;
  padding: 12px;
}

.interpretation p {
  font-size: 13px;
  line-height: 1.5;
  margin: 8px 0 0;
}

@media (max-width: 500px) {
  .summary-grid {
    grid-template-columns: minmax(0, 1fr);
  }
}
</style>
