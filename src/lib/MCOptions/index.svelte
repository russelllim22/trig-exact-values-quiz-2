

<script>
import { loop_guard } from "svelte/internal";

    export let correctClickFunc, wrongClickFunc, theOptions;

</script>

<div id="button-wrapper">
{#each theOptions as option}
    <button on:click = {(e)=>{(option.correct === 1) ? correctClickFunc(e) : wrongClickFunc(e)}}>
        {#if option.text[0] === "sqrt" && +option.text[2] > 1}
            <span class="ML__mathlive"><span class="ML__strut" style="height:1.55em;"></span><span class="ML__strut--bottom" style="height:2.24em;vertical-align:-0.68em;"></span><span translate="no" aria-hidden="true" class="ML__base"><span><span class="ML__cmr">​</span><span><span class="sizing nulldelimiter mopen"></span><span class="vlist mfrac"><span style="top:-0.67em;"><span><span class="ML__cmr">​</span>{(option.text[3] && option.text[3]==="neg")?"-":""}<span class="sqrt"><span class="sqrt-sign" style="top:-0.06em;"><span class="ML__small-delim style-wrap"><span>√</span></span></span><span class="vlist"><span><span><span class="ML__cmr">​</span><span class="ML__cmr">{option.text[1]}</span></span></span><span style="top:-0.82em;"><span class="sqrt-line"></span></span></span></span></span></span><span style="top:-0.22em;"><span class="frac-line"></span></span><span style="top:0.69em;"><span><span class="ML__cmr">​</span><span class="ML__cmr">{option.text[2]}</span></span></span></span><span class="sizing nulldelimiter mclose"></span></span></span></span></span>
        {:else if option.text[0] === "sqrt"}
        <span class="ML__mathlive"><span class="ML__strut" style="height:1.02em;"></span><span class="ML__strut--bottom" style="height:1.21em;vertical-align:-0.18em;"></span><span translate="no" aria-hidden="true" class="ML__base"><span><span class="ML__cmr">​</span>{(option.text[3] && option.text[3]==="neg")?"-":""}<span class="sqrt"><span class="sqrt-sign" style="top:-0.16em;"><span class="style-wrap"><span class="delimsizing size1">√</span></span></span><span class="vlist"><span><span><span class="ML__cmr">​</span><span class="ML__cmr">{option.text[1]}</span></span></span><span style="top:-0.97em;"><span class="sqrt-line"></span></span></span></span></span></span></span>
        {:else if option.text[0] === "frac"}
            <span class="ML__fieldcontainer__field"><span class="ML__mathlive"><span class="ML__strut" style="height:1.33em;"></span><span class="ML__strut--bottom" style="height:2.01em;vertical-align:-0.68em;"></span><span translate="no" aria-hidden="true" class="ML__base"><span><span class="ML__cmr">​</span><span><span class="sizing nulldelimiter mopen"></span><span class="vlist mfrac"><span style="top:-0.67em;"><span><span class="ML__cmr">​</span><span class="ML__cmr">{(option.text[3] && option.text[3]==="neg")?"-":""}{option.text[1]}</span></span></span><span style="top:-0.22em;"><span class="frac-line"></span></span><span style="top:0.69em;"><span><span class="ML__cmr">​</span><span class="ML__cmr">{option.text[2]}</span></span></span></span><span class="sizing nulldelimiter mclose"></span></span></span></span></span></span>
        {:else}
            <span style="font-family: KaTeX_Main">{option.text[0]}</span>
        {/if}
    </button>    
{/each}
</div>

<style>
    #button-wrapper{
        width:95%;
        position: absolute;
        bottom: 30px;
        display: flex;
		/*flex-direction: column;*/
		justify-content: center;
		align-items: center;
        flex-wrap: wrap;
    }
    button{
        font-size: 36px;
        width: 15vw;
        height: 12vw;
        margin: 2vw;
        cursor: pointer;
        border: 2px solid #26a0da;
        background: #2c2f33;
        border-radius: 10px;
        background-image: linear-gradient(to right, #314755 0%, #26a0da  51%, #314755  100%);
        padding: 10px;
        text-align: center;
        transition: 0.5s;
        background-size: 200% auto;
        color: white;            
    }
    @media only screen and (max-width: 600px),
    screen and (max-height: 600px) {
        button{
            font-size: 30px;
            margin: 10px;
        }
        #button-wrapper{
            bottom: 10px;
        }
    }
    @media only screen and (max-width: 600px){
        button{
            width: 30%;
            height: 90px;
            margin: 3%;
        }
    }
    button:hover {
        background-position: right center; /* change the direction of the change here */
        color: #fff;
        text-decoration: none;
    }    

    button .ML__base{
        cursor: pointer;
    }
    button:disabled{
        background : grey;
        border-color : darkgrey;
        color : darkgrey;
    }
    button {
        box-shadow: 0px 0px 0px goldenrod;
        transition: box-shadow 0.3s ease-out;
    }

</style>

