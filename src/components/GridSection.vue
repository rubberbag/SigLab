<script setup>
import {ref} from 'vue'
import TablE from './TablE.vue';
import charT from './charT.vue';

const summaryTitle = ref('Panel C [Summary]')
const harmonicsTitle = ref('Panel D [Harmonics]')
</script>


<template>
    <div class="container">
        <div class="column-1"> 
            <div class="chart">
                <div class="chart-content">
                    Panel A [Time Domain (Raw WaveForm)]
                    <charT/>
                </div>
            </div>
            <div class="chart">
                <div class="chart-content">
                    Panel B [Frequency Domain (Spectrum)] 
                    <charT/>
                </div>
            </div>
            <div class="chart">
                <div class="chart-content">
                    Panel E [Time-Frequency Spectrogram]
                    <charT/> 
                </div> 
            </div>
        </div>
        <div class="column-2">
            <div class="summary">
                <TablE :title="summaryTitle" class="table-content"/>
            </div>
            <div class="harmonics" >

                <TablE :title="harmonicsTitle" class="table-content"/>
            </div>
        </div>
    </div>
</template>

<style scoped>
    .container{
        display: grid;
        grid-template-columns: 3fr 1fr;
        gap: 10px;

        align-items: stretch;
    }

    .column-1, .column-2{
        gap: 20px;

    }

    .column-1{
        display: grid;
        grid-template-rows: repeat(3, 
        minmax(26.9rem,1fr));
    }

    .column-2{
        display: grid;
        grid-template-rows: 
        minmax(26.0rem, 1fr) 
        minmax(26.0rem, 2fr);
    }

    .chart-content,
    .summary,
    .harmonics{
        padding: 5px;   
        box-sizing: border-box;
        background:#f5f4f4; 
    }

    .chart-content{
        height: 100%;
        padding: 5px;
        border:1px dashed #525252;
        border-inline-end: 0;
        border-block-end: 0;
    }

</style>