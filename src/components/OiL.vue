<template>
    <div class="brand-label">
        <div class="brand">
            <span class="name">OSON</span>
            <span class="lname"> INDUSTRIAL LABORATORIES</span>
        </div>
        <div class="tagline">
            <span>METAL UP</span>
        </div>
    </div>
</template>

<style scoped>

.brand-label{
    width:35% ;
    display: flex;
    align-items:  flex-end;
}

.brand{
    width:auto ;
    height: 25px;
    display: flex;
    background: #a1a1a1; 
}

.name, .lname{
    display: flex;
    align-items: center;
    justify-content: center;
    box-sizing: border-box;

    padding-inline:10px;
    padding-block: 5px;

    color: #fff !important;

    font:20px;
    letter-spacing: 2px;
}

.name{
    width: auto;  
    background: #4502ff;   
}

.lname{
    width: auto;
    border-left: 2px solid #fff;
}

.tagline{
    width: auto;
    display:flex;

    font-size: 15px;
    padding-inline:2px ;
}

</style>