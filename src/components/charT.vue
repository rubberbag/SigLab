<template>
    <canvas ref="canvas"></canvas>
  </template>
  
  <script setup>
  import { ref, onMounted } from "vue";
  
  const canvas = ref(null);
  
  // Generic data — the chart doesn't care what it represents
  const data = [-2, -1, -0.5, 3, 5, 20, 6, 3, 2, 4, 15, 5, 3];
  
  onMounted(() => {
    const ctx = canvas.value.getContext("2d");
  
    const width = 1150;
    const height = 400;
  
    canvas.value.width = width;
    canvas.value.height = height;
  
    const padding = 20;
  
    const graphWidth = width - padding * 2;
    const graphHeight = height - padding * 2;
  
    // Find data range
    const min = Math.min(...data);
    const max = Math.max(...data);
  
    // Convert a data value to canvas Y position
    const getY = (value) => {
      return (
        height -
        padding -
        ((value - min) / (max - min)) * graphHeight
      );
    };
  
    // --------------------
    // AXES
    // --------------------
  
    ctx.strokeStyle = "black";
    ctx.lineWidth = 1;
  
    // X axis
    ctx.beginPath();
    ctx.moveTo(padding, height - padding);
    ctx.lineTo(width - padding, height - padding);
    ctx.stroke();
  
    // Y axis
    ctx.beginPath();
    ctx.moveTo(padding, padding);
    ctx.lineTo(padding, height - padding);
    ctx.stroke();
  
    // --------------------
    // DATA LINE
    // --------------------
  
    ctx.beginPath();
  
    data.forEach((value, i) => {
      const x =
        padding +
        (i / (data.length - 1)) * graphWidth;
  
      const y = getY(value);
  
      if (i === 0) {
        ctx.moveTo(x, y);
      } else {
        ctx.lineTo(x, y);
      }
    });
  
    ctx.strokeStyle = "blue";
    ctx.lineWidth = 2;
    ctx.stroke();
  });
  </script>
  
  <style scoped>
  canvas {
    display: block;
  }
  </style>
  