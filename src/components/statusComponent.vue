<template>
    <div class="status-bagde">
        <span class="hostname">{{"protocol"}}://{{ "host" }}:{{ "port" }}</span>
        <span class="status">DCNT</span>
    </div>
</template>

<style scoped>
.status-badge{
    width:400px ;
    height: 50px;
  
    overflow: hidden;
}

.hostname{
    width:250px;

    align-items: center;
    box-sizing: border-box;

    padding-inline:10px;
    padding-block: 2px;

    background: #686868;
    font : 20px;
    letter-spacing: 1px;

    color: #fff !important;
    letter-spacing: 2px;
}

.status{
    width: 150px;
    align-items: center;
    justify-content: center;
    box-sizing:  border-box;

    padding-inline:5px;
    padding-block: 2px;

    background: red;
    color:#fff !important;
    border-left: 2px solid #aaa;

    font: 20px;
    font-weight: 700;
    letter-spacing: 2px;
}
</style>