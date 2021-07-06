<script>
	import { tweened } from 'svelte/motion';
	import { cubicOut } from 'svelte/easing';

    export let currentAngle, showAngle, showLength;
    $: { if(+currentAngle > 360){currentAngle = currentAngle - 360;}
         else if(+currentAngle === 0){currentAngle = 0.1;} }
    
    const lengthTween = tweened(0, {
		duration: 400,
		easing: cubicOut
	});
    
    $: { if(["sin","cos","tan"].includes(showLength)){
            lengthTween.set( 1 );
        }
        else{
            lengthTween.set( 0 );
        }  
    }
    
</script>

<div>
<svg viewBox="0 0 225 225" preserveAspectRatio= "">
    <defs>
        <marker id="arrow" fill="#4d5054" markerWidth="10" markerHeight="10" refX="5" refY="5" orient="auto">
          <polygon points="0 0, 10 5, 0 10" />
        </marker>
        <marker id="gold-arrow" fill="goldenrod" markerWidth="5" markerHeight="5" refX="2.5" refY="2.5" orient="auto">
            <polygon points="0 0, 5 2.5, 0 5" />
          </marker>
        <marker id="endpoint" markerWidth="8" markerHeight="8" refX="4" refY="4" orient="auto">
            <circle r=4 cx=4 cy=4></circle>
        </marker>
        <marker id="gold-point" markerWidth="5" markerHeight="5" refX="2.5" refY="2.5" orient="auto">
            <circle r=2.5 cx=2.5 cy=2.5></circle>
        </marker>
      </defs>
    <circle r=100 cx=110 cy=115></circle>
    <line x1="0" y1="115" x2="220" y2="115" stroke="#4d5054" stroke-width="1" marker-end="url(#arrow)" />
    <line x1="110" y1="225" x2="110" y2="5" stroke="#4d5054" stroke-width="1" marker-end="url(#arrow)" />
    <line x1="110" y1="115" x2="{Math.cos(currentAngle * Math.PI / 180) * 100 + 110}" y2="{-Math.sin(currentAngle * Math.PI / 180) * 100 + 115}" stroke="#26a0da" stroke-width="1" marker-end="url(#endpoint)" style="opacity: {(showAngle) ? 1 : 0}"/> 
    <line x1="{Math.cos(currentAngle * Math.PI / 180) * 100 + 110}" y1="115" x2="{Math.cos(currentAngle * Math.PI / 180) * 100 + 110}" y2="{-Math.sin(currentAngle * Math.PI / 180) * 100 + 115}" stroke="#26a0da" stroke-width="1" stroke-dasharray="2px" style="opacity: {(["sin","cos"].includes(showLength)) ? 1 : 0}"/>
    <line id="cosArrow" x1="110" y1="115" x2="{$lengthTween * Math.cos(currentAngle * Math.PI / 180) * 100 + 110}" y2="115" stroke="goldenrod" stroke-width="3" marker-end="{[-270,-90,90,270,450].includes(parseInt(currentAngle)) ? "url(#gold-point)" : "url(#gold-arrow)"}" style="opacity: {(showLength === "cos") ? 1 : 0}"/> 
    <line id="sinArrow" x1="{Math.cos(currentAngle * Math.PI / 180) * 100 + 110}" y1="115" x2="{Math.cos(currentAngle * Math.PI / 180) * 100 + 110}" y2="{$lengthTween * -Math.sin(currentAngle * Math.PI / 180) * 100 + 115}" stroke="goldenrod" stroke-width="3" marker-end="{[0,-180,180,360,540].includes(parseInt(currentAngle)) ? "url(#gold-point)" : "url(#gold-arrow)"}" style="opacity: {(showLength === "sin") ? 1 : 0}"/> 
    <line id="tanArrow" x1="210" y1="115" x2="210" y2="{$lengthTween * -Math.tan(currentAngle * Math.PI / 180) * 100 + 115}" stroke="goldenrod" stroke-width="3" marker-end="{[0,-180,180,360,540].includes(parseInt(currentAngle)) ? "url(#gold-point)" : "url(#gold-arrow)"}" style="opacity: {(showLength === "tan") ? 1 : 0}"/> 
    <line id="tanDashLine" x1="110" y1="115" x2="210" y2="{-Math.tan(currentAngle * Math.PI / 180) * 100 + 115}" stroke="#26a0da" stroke-width="1" stroke-dasharray="2px" style="opacity: {(showLength === "tan") ? 1 : 0}"/> 
    
    <circle id="angle-sector" cx="110" cy="115" r=10 style="stroke: #26a0da; stroke-dashoffset: {-2 * Math.PI * 10 + Math.PI * 10 * currentAngle / 180 }px; opacity: {(showAngle) ? 1 : 0}"></circle>
</svg>
</div>

<style>
    div{
        width: 50vh;
        position: absolute;
        top: calc(41% - 25vh);
        left: calc(50% - 25vh);
    }
    @media only screen and (max-width: 600px){
        div{
            width: 70%;
            margin: 10px;
            top: calc(50% - 35%);
            left: calc(50% - 35%);
        }
    }
    svg{
        overflow: visible;
    }
    svg circle{
        stroke: #4d5054;
        fill: none;
    }
    #endpoint circle{
        fill: #26a0da;
        stroke: none;
    }
    #gold-point circle{
        fill: goldenrod;
        stroke: none;
    }
    #angle-sector {
        stroke-dasharray: 62.83px;
        stroke-dashoffset: -52.36px;  /* 2pi * 10 - pi * 10 / 3 */
        stroke-width: 1px;
        stroke: grey;
        fill: none;
    }
</style>
