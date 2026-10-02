<template>
  <table class="reportTable">

    <tbody class="summary">
      <tr>
        <td colspan="6">
          <table>
            <tbody class="inner-tbody">
              <tr>
                  <td>
                    <div class="data-row">
                      <span>Signal Name</span>
                      <span>:</span>
                      <span>{{ "N/A" }}</span>
                    </div>
                    <div class="data-row">
                      <span>Signal Type</span>
                      <span>:</span>
                      <span>{{ "N/A" }}</span>
                    </div>
                    <div class="data-row">
                      <span>Signal Id.</span>
                      <span>:</span>
                      <span>{{ "N/A" }}</span>
                    </div>
                  </td>

                  <td>
                    <div class="data-row">
                      <span>Sample Rate</span>
                      <span>:</span>
                      <span>{{ "N/A" }}</span>
                    </div>
                    <div class="data-row">
                      <span>Sample Time</span>
                      <span>:</span>
                      <span>{{ "N/A" }}</span>
                    </div>
                    <div class="data-row">
                      <span>Sample No.</span>
                      <span>:</span>
                      <span>{{ "N/A" }}</span>
                    </div>
                  </td>

                  <td>
                    <div class="data-row">
                      <span>Machine</span>
                      <span>:</span>
                      <span>{{ "N/A" }}</span>
                    </div>
                    <div class="data-row">
                      <span>Sensor</span>
                      <span>:</span>
                      <span>{{ "N/A" }}</span>
                    </div>
                    <div class="data-row">
                      <span>Unit</span>
                      <span>:</span>
                      <span>{{ "N/A" }}</span>
                    </div>
                  </td>
                </tr>
            </tbody>
          </table>
        </td>
      </tr>
    </tbody>

    <tbody class="sample-summary">
      <tr>
        <th>Harmonics</th>
        <th>Imbalance</th>
        <th>Bearing Fault</th>
        <th>Noise</th>
        <th>Drift</th>
        <th>Electric Interference</th>
      </tr>

      <tr>
        <td>{{ "N/A" }}</td>
        <td>{{ "N/A" }}</td>
        <td>{{ "N/A" }}</td>
        <td>{{ "N/A" }}</td>
        <td>{{ "N/A" }}</td>
        <td>{{ "N/A" }}</td>
      </tr>
    </tbody>

  </table>
</template>


<style scoped>
table.reportTable{
  border: 1px solid;
}

table{
  width: 100%;
  table-layout: fixed;
  border-collapse: collapse;
}

.summary > tr > td{
  justify-items:stretch;
}

.inner-tbody> tr >td:nth-child(2){
  border-inline: 1px solid;
}

.inner-tbody > tr >td{
  padding:5px;
}

.data-row{
  display: flex;
  width:100%;
}

.data-row > span:first-child {
  width: 20%;
}

.data-row > span:nth-child(2) {
  width: 5%;
}


.data-row > span:nth-child(3) {
  width: 75%;
}

th{
  font-weight:normal;
  border-block:1px solid;
  text-align: left;
}

.sample-summary > tr >th , .sample-summary > tr >td {
  padding: 5px;
}

</style>