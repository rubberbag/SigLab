<template>
  <div class="status-badge">
    <span class="hostname">ws://{{ url }}</span>
    <span class="status" :class="{ connected }">
      {{ connected ? 'CNTD' : 'DCNT' }}
    </span>
  </div>
</template>

<script setup>
defineProps({
  url: {
    type: String,
    default: 'localhost:XXXX'
  },
  connected: {
    type: Boolean,
    default: false
  }
})
</script>

<style scoped>
.status-badge {
  display: flex;
  width: auto;
  height: 25px;
  overflow: hidden;
}

.hostname {
  width: 250px;
  padding: 2px 10px;
  box-sizing: border-box;
  background: #686868;
  color: white;
  font-size: 16px;
  letter-spacing: 1px;
  white-space: nowrap;
  overflow: hidden;
}

.status {
  display: flex;
  align-items: center;
  justify-content: center;
  width: 150px;
  padding: 2px 5px;
  box-sizing: border-box;
  background: red;
  color: white;
  border-left: 2px solid #aaa;
  font-size: 16px;
  font-weight: 700;
}

.status.connected {
  background: green;
}
</style>
