<script setup>
import OiL from './components/OiL.vue';
import SummaryTable from './components/SummaryTable.vue';
import statusComponent from './components/statusComponent.vue';
import TdFeatureExtraction from './components/TdFeatureExtraction.vue';
import GridSection from './components/GridSection.vue';
</script>

<template>

  <div class="brand">
    <OiL/>
  </div>

  <div class="title">
    <h1>Rotary Machine Signal Processing Analysis</h1>
  </div>

  <section class="summaryTable">
    <SummaryTable/>
  </section>

  <div class="statusComponent">
    <statusComponent/>
  </div>

  <div class="tdfe">
    <TdFeatureExtraction/>
  </div>

  <section class="gridSection">
    <GridSection/>
  </section>

</template>

<style scoped>

.brand,.title, .summaryTable, .statusComponent, .tdfe, .gridSection{
  margin-top: 20px;
}


.title h1{
  text-align: center;
  padding-inline:35px;
  padding-block:3px;
}

.tdfe{
  width: 100%;
  display: flex;
  justify-content:flex-end;
  box-sizing: border-box;
}
</style>
