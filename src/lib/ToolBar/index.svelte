<script>
    export let angleTypes, trigFunctions, angleQuadrants, settingChanged, toolBarOpen;
    let updateTrigFunctions = (e)=>{  
        if(e.target.value.includes(",")){
            trigFunctions = e.target.value.split(",");
        }
        else{
            trigFunctions = [e.target.value];
        }
        settingChanged = true;
    }
    let updateAngleTypes = (e)=>{  
        if(e.target.value.includes(",")){
            angleTypes = e.target.value.split(",");
        }
        else{
            angleTypes = [e.target.value];
        }
        settingChanged = true;
    }
    let updateAngleQuads = (e)=>{
        if(e.target.value.includes(",")){
            angleQuadrants = e.target.value.split(",");
        }
        else{
            angleQuadrants = [e.target.value];
        }
        settingChanged = true;
    }
</script>

<div id="toolbar" style="width: {(toolBarOpen ? "95%" : "50px")}">
    <div id="settings-icon-div" on:click="{()=>{toolBarOpen = !toolBarOpen;}}">
            <svg class="settings" xmlns="http://www.w3.org/2000/svg" viewBox="18 18 64 64">
                <path d="M79.649,54.255v-8.509l-8.053-1.832c-0.484-1.721-1.162-3.358-2.019-4.885l4.396-6.986l-6.017-6.017l-6.985,4.396c-1.526-0.859-3.164-1.536-4.885-2.02l-1.832-8.051h-8.509l-1.832,8.051c-1.72,0.485-3.357,1.161-4.886,2.02l-6.986-4.396l-6.017,6.017l4.397,6.986c-0.858,1.527-1.536,3.164-2.021,4.885l-8.052,1.832v8.509l8.052,1.831c0.484,1.721,1.162,3.358,2.021,4.885l-4.397,6.987l6.017,6.017l6.986-4.396c1.527,0.858,3.164,1.535,4.886,2.02l1.832,8.051h8.509l1.832-8.051c1.72-0.484,3.357-1.161,4.885-2.02l6.985,4.396l6.017-6.017l-4.396-6.987c0.858-1.526,1.534-3.164,2.019-4.885L79.649,54.255z" fill="none" stroke-width="3" stroke="white"></path>
                <path d="M50,35.171c8.189,0,14.829,6.639,14.829,14.83c0,8.191-6.64,14.83-14.829,14.83c-8.191,0-14.83-6.639-14.83-14.83C35.17,41.81,41.809,35.171,50,35.171z" fill="none" stroke-width="3" stroke="white"></path>
            </svg>
    </div>
    <div class="toolbar-option" style="display: {(toolBarOpen ? "inline" : "none")}">
    <label for="trig-functions">Functions:</label>
    <!-- svelte-ignore a11y-no-onchange -->
    <select id="trig-functions" on:change = {(e)=>{updateTrigFunctions(e)}}>
        <option value="sin">sine</option>
        <option value="sin,cos">sine, cosine</option>
        <option value="sin,cos,tan" selected>sine, cosine, tangent</option>
    </select>
    </div>
    <div class="toolbar-option" style="display: {(toolBarOpen ? "inline" : "none")}">
    <label for="angle-units">Angle units:</label>
    <!-- svelte-ignore a11y-no-onchange -->
    <select id="angle-units" on:change = {(e)=>{updateAngleTypes(e)}}>
        <option value="degrees">degrees</option>
        <option value="radians">radians</option>
        <option value="degrees,radians,radians" selected>degrees & radians</option>
    </select>
    </div>
    <div class="toolbar-option" style="display: {(toolBarOpen ? "inline" : "none")}">
    <label for="angle-quadrants">Angle quadrants:</label>
    <!-- svelte-ignore a11y-no-onchange -->
    <select id="angle-quadrants" on:change = {(e)=>{updateAngleQuads(e)}}>
        <option value = "Q1">Quadrant 1</option>
        <option value = "Q1,Q2">Quadrants 1 and 2</option>
        <option selected value = "Q1,Q2,Q3,Q4">All Quadrants</option>
        <option value = "Q1,Q2,Q3,Q4,neg">All + negatives</option>
    </select>
    </div>
</div>

<style>
    #toolbar{
        width:50px;
        min-height: 50px;
        position: absolute;
        top: 10px;
        left: 10px;
        background-color: #5c5c5c;
        border: 1px solid white;
        border-radius: 5px;
        padding: 5px;
        color: white;
        font-size: 16px;
        display: flex;
		flex-direction: row;
        flex-wrap: wrap;
		justify-content: center;
		align-items: center;
        z-index: 2;
        transition: width 0.25s ease-out;
    }
    .toolbar-option{
        margin: 10px;
    }
    #settings-icon-div{
        width: 30px;
        cursor: pointer;
    }
    select{
        color: white;
        background-color: #5c5c5c;
        font-size: 16px;
    }
</style>