<script setup>
import TablE from './TablE.vue'
import TimeFrequencySummary from './TimeFrequencySummary.vue';
import charT from './charT.vue'
import SpectrograM from './spectrograM.vue';
import { useSignalData} from '@/composables/useSignalData.js';

const {
  waveformPoints,
  fftPoints,
  spectrogramTimes,
  spectrogramFrequencies,
  spectrogramMagnitude
} = useSignalData()


</script>

<template>
  <div class="container">
    <!-- Row 1, Column 1 -->
    <div class="panel chart-content">
      <div class="panel-title">
        Panel A [Time Domain (Raw Waveform)]
      </div>
      <charT
        :points="waveformPoints"
        x-label="Time (s)"
        y-label="Amplitude"
      />
    </div>

    <!-- Row 1, Column 2 -->
    <div class="panel">
      <TablE title="Panel B [Time Domain Values]" />
    </div>

    <!-- Row 2, Column 1 -->
    <div class="panel chart-content">
      <div class="panel-title">
        Panel C [Frequency Domain (Spectrum)]
      </div>
      <charT
        :points="fftPoints"
        x-label="Frequency (Hz)"
        y-label="Amplitude"
        :x-min="0"
        :x-max="10"
        :y-min="-10"
        :y-max="10"
        color="#ed8735"
      />
    </div>

    <!-- Row 2, Column 2 -->
    <div class="panel">
      <TablE title="Panel D [Harmonics Peak]" />
    </div>

    <!-- Row 3, Column 1 -->
    <div class="panel chart-content">
      <div class="panel-title">
        Panel E [Time-Frequency Spectrogram]
      </div>
      <SpectrograM
        :times="spectrogramTimes"
        :frequencies="spectrogramFrequencies"
        :magnitude="spectrogramMagnitude"
      />
    </div>

    <!-- Row 3, Column 2 -->
    <div class="panel">
      <TimeFrequencySummary title="Panel F [Summary]" />
    </div>
  </div>
</template>

<style scoped>
.container {
  display: grid;
  grid-template-columns: minmax(0, 3fr) minmax(0, 2fr);
  grid-template-rows: repeat(3, minmax(26rem, 1fr));
  gap: 12px;
  width: 100%;
  min-width: 0;
}

.panel {
  min-width: 0;
  min-height: 0;
  box-sizing: border-box;
  padding: 5px;
  overflow: hidden;
}

.chart-content {
  display: flex;
  flex-direction: column;
  border: 1px dashed #525252;
  border-inline-end: 0;
  border-block-end: 0;
}

.panel-title {
  flex: 0 0 auto;
  margin-bottom: 5px;
  font-size: 14px;
}

.chart-content :deep(.chart-container) {
  flex: 1 1 0;
  height: 0;
  min-height: 0;
  min-width: 0;
}

@media (max-width: 850px) {
  .container {
    grid-template-columns: minmax(0, 1fr);
    grid-template-rows: none;
    grid-auto-rows: minmax(22rem, auto);
  }
}
</style>
