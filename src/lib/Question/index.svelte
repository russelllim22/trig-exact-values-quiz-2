<script>
    import { fade, fly } from 'svelte/transition';
    export let currentQuestion;
</script>

{#if currentQuestion.length > 0}
    <p id="question" in:fly="{{ x: -0.3*window.innerWidth, y: 0.1*window.innerWidth, duration: 500,}}"  out:fly="{{x:0.3*window.innerWidth, y: 0.1*window.innerWidth, duration: 500, delay: 500 }}">
        {#if currentQuestion[1].includes("pi") && currentQuestion[2]} <!-- fraction of pi -->
            <span class="ML__mathlive"><span class="ML__strut" style="height:1.2em;"></span><span class="ML__strut--bottom" style="height:1.89em;vertical-align:-0.68em;"></span><span translate="no" aria-hidden="true" class="ML__base"><span><span class="ML__cmr">​</span><span class="ML__cmr">{currentQuestion[0]}</span><span class="ML__open style-wrap"><span class="delimsizing size2">(</span></span><span class="ML__cmr">​</span><span><span class="sizing nulldelimiter mopen"></span><span class="vlist mfrac"><span style="top:-0.67em;"><span><span class="ML__cmr">​</span><span class="ML__cmr">{(currentQuestion[1].split("pi")[0] === "1") ? "" : (currentQuestion[1].split("pi")[0] === "-1") ? "-" : currentQuestion[1].split("pi")[0]}</span><span class="lcGreek ML__mathit" style="margin-right:0.04em;">π</span></span></span><span style="top:-0.22em;"><span class="frac-line"></span></span><span style="top:0.69em;"><span><span class="ML__cmr">​</span><span class="ML__cmr">{currentQuestion[2]}</span></span></span></span><span class="sizing nulldelimiter mclose"></span></span><span class="ML__close style-wrap"><span class="delimsizing size2">)</span></span></span></span></span>
        {:else if currentQuestion[1].includes("pi")} <!-- whole multiple of pi -->
            <span class="ML__mathlive"><span class="ML__strut" style="height:0.75em;"></span><span class="ML__strut--bottom" style="height:1em;vertical-align:-0.25em;"></span><span translate="no" aria-hidden="true" class="ML__base"><span><span class="ML__cmr">​</span><span class="ML__cmr">{currentQuestion[0]}</span><span class="ML__small-delim ML__open style-wrap"><span>(</span></span><span class="ML__cmr">​</span><span class="ML__cmr">{(currentQuestion[1].split("pi")[0] === "1") ? "" : (currentQuestion[1].split("pi")[0] === "-1") ? "-" : currentQuestion[1].split("pi")[0]}</span><span class="lcGreek ML__mathit" style="margin-right:0.04em;">π</span><span class="ML__small-delim ML__close style-wrap"><span>)</span></span></span></span></span> 
        {:else if currentQuestion[1] === "0"}
            <span class="ML__mathlive"><span class="ML__strut" style="height:0.75em;"></span><span class="ML__strut--bottom" style="height:1em;vertical-align:-0.25em;"></span><span translate="no" aria-hidden="true" class="ML__base"><span><span class="ML__cmr">​</span><span class="ML__cmr">{currentQuestion[0]}</span><span class="ML__small-delim ML__open style-wrap"><span>(</span></span><span class="ML__cmr">​</span><span class="ML__cmr">0</span><span class="ML__small-delim ML__close style-wrap"><span>)</span></span></span></span></span>
        {:else} <!-- angle in degrees -->
            <span class="ML__fieldcontainer__field"><span class="ML__mathlive"><span class="ML__strut" style="height:0.75em;"></span><span class="ML__strut--bottom" style="height:1em;vertical-align:-0.25em;"></span><span translate="no" aria-hidden="true" class="ML__base"><span><span class="ML__cmr">​</span><span class="ML__cmr">{currentQuestion[0]}</span><span class="ML__small-delim ML__open style-wrap"><span>(</span></span><span class="ML__cmr">​</span><span class="ML__cmr">{currentQuestion[1]}</span><span class="ML__cmr">°</span><span class="ML__small-delim ML__close style-wrap"><span>)</span></span></span></span></span></span>
        {/if}
    </p>
{/if}

<style>
    #question{
        width: 33%;
        min-width: 200px;
        text-align: center;
        font-size: 60px;
        color: white;
        margin: 30px;
    }
    @media only screen and (max-width: 600px),
    screen and (max-height: 600px) {
        #question{
            font-size: 40px;
            margin: 10px;
        }
    }
    #question .ML__mathlive{
        text-shadow: 0 0 goldenrod;
        transition: text-shadow 0.3s ease-out;
    }
</style>