<template>
    <table class="td-metrics">
        <thead class="td-head">
            <tr class="table-title">
                <th colspan="4">[Time-Domain Feature Extraction]</th>
            </tr>
            <tr class="table-head">
                <th>RMS</th>
                <th>Peak</th>
                <th>Crest Factor</th>
                <th>Kurtosis</th>
            </tr>
        </thead>
        <tbody class="td-body">
            <tr>
                <td>{{ "N/A" }}</td>
                <td>{{ "N/A" }}</td>
                <td>{{ "N/A" }}</td>
                <td>{{ "N/A" }}</td>
            </tr>
        </tbody>
    </table>
</template>

<style scoped>

.td-metrics{
    width: 50%;
    table-layout: fixed;
    border-collapse: collapse;
}

thead{
    text-align: left;
}

.table-title th{
    padding-inline: 5px;
    padding-block: 2px;
    border:1px solid;
    border-inline-end: 0;
}

.table-head th{
    border-inline:1px solid;
    padding:5px;
    font-weight: normal;
}

.td-body td{
    padding: 5px;
    border:1px solid;
}

</style>