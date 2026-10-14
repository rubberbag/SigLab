<template>
  <div class="chart-container">
    <div v-if="title" class="chart-title">{{ title }}</div>

    <canvas
      ref="canvas"
      @pointerdown="startPan"
      @pointermove="pan"
      @pointerup="endPan"
      @pointercancel="endPan"
      @wheel.prevent="zoom"
    ></canvas>

    <div class="toolbar">
      <button @click="resetView">Reset view</button>
      <button @click="resumeFollow">Follow latest</button>
    </div>
  </div>
</template>

<script setup>
import { ref, onMounted, onBeforeUnmount, watch } from "vue";

const props = defineProps({
  title: { type: String, default: "" },

  // Generic points: [{ x: 0, y: 2 }, { x: 1, y: 5 }]
  points: { type: Array, default: () => [] },

  xLabel: { type: String, default: "Time" },
  yLabel: { type: String, default: "Amplitude" },
  color: { type: String, default: "#1683df" },

  xMin: { type: Number, default: 0 },
  xMax: { type: Number, default: 10 },
  yMin: { type: Number, default: -10 },
  yMax: { type: Number, default: 10 },

  autoFollow: { type: Boolean, default: false },
});

const canvas = ref(null);

let ctx;
let observer;
let width = 0;
let height = 0;
let dragging = false;
let lastPointer = null;
let following = props.autoFollow;

const margin = { left: 55, right: 15, top: 15, bottom: 40 };

const view = {
  xMin: props.xMin,
  xMax: props.xMax,
  yMin: props.yMin,
  yMax: props.yMax,
};

function resize() {
  if (!canvas.value) return;

  const rect = canvas.value.getBoundingClientRect();
  const dpr = window.devicePixelRatio || 1;

  width = rect.width;
  height = rect.height;

  canvas.value.width = Math.round(width * dpr);
  canvas.value.height = Math.round(height * dpr);

  ctx = canvas.value.getContext("2d");
  ctx.setTransform(dpr, 0, 0, dpr, 0, 0);

  draw();
}

function px(x) {
  return margin.left +
    ((x - view.xMin) / (view.xMax - view.xMin)) *
    (width - margin.left - margin.right);
}

function py(y) {
  return margin.top +
    ((view.yMax - y) / (view.yMax - view.yMin)) *
    (height - margin.top - margin.bottom);
}

function tickStep(span) {
  const rough = span / 18;
  const power = 10 ** Math.floor(Math.log10(rough || 1));
  const fraction = rough / power;

  return (fraction <= 1 ? 1 : fraction <= 2 ? 2 :
    fraction <= 5 ? 5 : 10) * power;
}

function draw() {
  if (!ctx || !width || !height) return;

  ctx.clearRect(0, 0, width, height);
  ctx.fillStyle = "#fff";
  ctx.fillRect(0, 0, width, height);

  const plotWidth = width - margin.left - margin.right;
  const plotHeight = height - margin.top - margin.bottom;

  // Grid
  ctx.strokeStyle = "#dedede";
  ctx.fillStyle = "#555";
  ctx.lineWidth = 1;
  ctx.font = "11px Arial";
  ctx.setLineDash([4, 4]);

  const xStep = tickStep(view.xMax - view.xMin);
  const yStep = tickStep(view.yMax - view.yMin);

  for (
    let x = Math.ceil(view.xMin / xStep) * xStep;
    x <= view.xMax;
    x += xStep
  ) {
    const p = px(x);

    ctx.beginPath();
    ctx.moveTo(p, margin.top);
    ctx.lineTo(p, height - margin.bottom);
    ctx.stroke();

    ctx.textAlign = "center";
    ctx.textBaseline = "top";
    ctx.fillText(Number(x.toPrecision(4)), p, height - margin.bottom + 6);
  }

  for (
    let y = Math.ceil(view.yMin / yStep) * yStep;
    y <= view.yMax;
    y += yStep
  ) {
    const p = py(y);

    ctx.beginPath();
    ctx.moveTo(margin.left, p);
    ctx.lineTo(width - margin.right, p);
    ctx.stroke();

    ctx.textAlign = "right";
    ctx.textBaseline = "middle";
    ctx.fillText(Number(y.toPrecision(4)), margin.left - 6, p);
  }

  // Plot boundary
  ctx.setLineDash([]);
  ctx.strokeStyle = "#333";
  ctx.strokeRect(margin.left, margin.top, plotWidth, plotHeight);

  // Clip data to plot area
  ctx.save();
  ctx.beginPath();
  ctx.rect(margin.left, margin.top, plotWidth, plotHeight);
  ctx.clip();

  ctx.beginPath();
  ctx.strokeStyle = props.color;
  ctx.lineWidth = 1.8;

  let started = false;

  for (const point of props.points) {
    if (
      !Number.isFinite(point.x) ||
      !Number.isFinite(point.y) ||
      point.x < view.xMin ||
      point.x > view.xMax
    ) {
      started = false;
      continue;
    }

    const x = px(point.x);
    const y = py(point.y);

    if (!started) {
      ctx.moveTo(x, y);
      started = true;
    } else {
      ctx.lineTo(x, y);
    }
  }

  ctx.stroke();
  ctx.restore();

  // Axis titles
  ctx.fillStyle = "#222";
  ctx.font = "12px Arial";
  ctx.textAlign = "center";

  ctx.fillText(
    props.xLabel,
    margin.left + plotWidth / 2,
    height - 4
  );

  ctx.save();
  ctx.translate(13, margin.top + plotHeight / 2);
  ctx.rotate(-Math.PI / 2);
  ctx.fillText(props.yLabel, 0, 0);
  ctx.restore();
}

// Drag to pan in both directions
function startPan(event) {
  dragging = true;
  following = false;

  lastPointer = {
    x: event.offsetX,
    y: event.offsetY,
  };

  canvas.value.setPointerCapture(event.pointerId);
}

function pan(event) {
  if (!dragging || !lastPointer) return;

  const dx = event.offsetX - lastPointer.x;
  const dy = event.offsetY - lastPointer.y;

  const plotWidth = width - margin.left - margin.right;
  const plotHeight = height - margin.top - margin.bottom;

  const xSpan = view.xMax - view.xMin;
  const ySpan = view.yMax - view.yMin;

  view.xMin -= dx / plotWidth * xSpan;
  view.xMax -= dx / plotWidth * xSpan;

  view.yMin += dy / plotHeight * ySpan;
  view.yMax += dy / plotHeight * ySpan;

  lastPointer = { x: event.offsetX, y: event.offsetY };

  draw();
}

function endPan() {
  dragging = false;
  lastPointer = null;
}

// Zoom around the mouse position
function zoom(event) {
  const rect = canvas.value.getBoundingClientRect();

  const mouseX = event.clientX - rect.left;
  const mouseY = event.clientY - rect.top;

  const anchorX = view.xMin +
    ((mouseX - margin.left) /
      (width - margin.left - margin.right)) *
    (view.xMax - view.xMin);

  const anchorY = view.yMax -
    ((mouseY - margin.top) /
      (height - margin.top - margin.bottom)) *
    (view.yMax - view.yMin);

  const factor = event.deltaY < 0 ? 0.85 : 1.18;

  view.xMin = anchorX - (anchorX - view.xMin) * factor;
  view.xMax = anchorX + (view.xMax - anchorX) * factor;

  view.yMin = anchorY - (anchorY - view.yMin) * factor;
  view.yMax = anchorY + (view.yMax - anchorY) * factor;

  following = false;
  draw();
}

function resetView() {
  view.xMin = props.xMin;
  view.xMax = props.xMax;
  view.yMin = props.yMin;
  view.yMax = props.yMax;

  following = props.autoFollow;
  draw();
}

function resumeFollow() {
  following = true;
  followLatest();
}

function followLatest() {
  const last = props.points[props.points.length - 1];
  if (!last) return;

  const span = view.xMax - view.xMin;

  view.xMax = last.x;
  view.xMin = last.x - span;

  draw();
}

watch(
  () => props.points,
  () => {
    if (following) followLatest();
    else draw();
  },
  { deep: true }
);

onMounted(() => {
  resize();

  observer = new ResizeObserver(resize);
  observer.observe(canvas.value);
});

onBeforeUnmount(() => {
  observer?.disconnect();
});

</script>

<style scoped>
.chart-container {
  width: 100%;
  height: 100%;
  min-width: 0;
  display: flex;
  flex-direction: column;
}

.chart-title {
  font-size: 13px;
  margin: 3px 4px 6px;
  color: #444;
}

canvas {
  display: block;
  flex: 1;
  width: 100%;
  min-height: 180px;
  touch-action: none;
  cursor: crosshair;
}

.toolbar {
  display: flex;
  gap: 5px;
  padding-top: 6px;
}

button {
  font-size: 11px;
  padding: 5px 8px;
  border: 1px solid #ddd;
  background: #fff;
  cursor: pointer;
}
</style>
