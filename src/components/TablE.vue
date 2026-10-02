<template>
    <table class="generic-table">
        <thead>
            <tr class="table-row">
                <th colspan="2">
                    {{ title }}
                </th>
            </tr>
        </thead>
        <tbody>
            <tr class="table-row">
                <td>{{ "N/A" }}</td>
                <td>{{ "N/A" }}</td>
            </tr>
            <tr class="table-row">
                <td>{{ "N/A" }}</td>
                <td>{{ "N/A" }}</td>
            </tr>
            <tr class="table-row">
                <td>{{ "N/A" }}</td>
                <td>{{ "N/A" }}</td>
            </tr>
            <tr class="table-row">
                <td>{{ "N/A" }}</td>
                <td>{{ "N/A" }}</td>
            </tr>
            <tr class="table-row">
                <td>{{ "N/A" }}</td>
                <td>{{ "N/A" }}</td>
            </tr>
        </tbody>
    </table>
</template>

<script setup>
defineProps({
    title:{
        type:String,
        default: 'Default Table Title'
    }
})
</script>

<style scoped>

.generic-table{
    width: 100%;
    table-layout: fixed;
    border-collapse: collapse;
}

.table-row th{
    padding: 5px;
    text-align: left;
    border:1px solid;
    border-inline-end: 0;
}

.table-row td{
    padding: 10px;
    border: 1px solid;
    text-align: center;
}

</style>