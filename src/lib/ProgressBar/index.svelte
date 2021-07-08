<script>
    import { fade } from 'svelte/transition';
    import { scale } from 'svelte/transition';
	import { tweened } from 'svelte/motion';
	import { cubicOut } from 'svelte/easing';
    export let currentScore;
    let starArray = [];

	const progress = tweened(0, {
		duration: 400,
		easing: cubicOut
	});

    $: {
        progress.set( currentScore / 20 * 100);
        if(currentScore === 19){
            for(let i=0; i<window.innerWidth/15; i++){
                starArray.push({"x":0.98*window.innerWidth*Math.random() , "y":0.9*window.innerHeight*Math.random()});
            }
        }
}

    
</script>

<svg id="progress-bar-svg">
    <defs>
        <path id="thumbs-up-path" d="M0 420.1h200v533.3H0zM996.5 573.7c-4.1-15-11.6-28.8-22-40.4 36.8-41.2 33.3-104.4-7.9-141.2-18.3-16.4-42.1-25.5-66.7-25.5H613.1l24.2-30.1c19-23.7 29.4-53.2 29.4-83.5V100c0-36.8-29.8-66.7-66.7-66.7-40.9 0-78.4 22.9-97.1 59.3L438 219.1 266.7 366.7v466.7c0 73.6 59.7 133.3 133.3 133.3h366.7c55.3 0 100-44.9 100-100.1 0-5.1-.4-10.3-1.2-15.3.8-5.9 1.2-11.9 1.2-17.9v-5.7c52.1-18.5 79.4-75.7 60.9-127.8l-1.2-3.3c53.2-14.6 84.6-69.6 70.1-122.9z"/>
        <radialGradient id="RadialGradient1">
            <stop offset="0%" stop-color="white" />
            <stop offset="60%" stop-color="white" />
            <stop offset="80%" stop-color="gold" />
            <stop offset="100%" stop-color="goldenrod" />
          </radialGradient>
          <linearGradient id="LinearGradient" x1="0" x2="100%" y1="0" y2="0" gradientUnits="objectBoundingBox">
            <stop offset="0%" stop-color="white" opacity-stop="0"/>
            <stop offset="50%" stop-color="gold" opacity-stop="1"/>
            <stop offset="100%" stop-color="white" opacity-stop="0"/>
          </linearGradient>
    </defs>
    <rect id="progress-bar" height="100%" width="10px" rx=2.5 x=25></rect>
    <rect id="progress" height="{Math.min($progress,100)}%" x=25 y="calc(100% - {Math.min($progress,100)}%)" width="10px" rx=2.5></rect>
    <line class="benchmark" x1=-10 y1="calc(50%)" x2=35 y2="calc(50%)" stroke="{(currentScore>=10) ? "goldenrod" : "grey"}"></line>
    <line class="benchmark" x1=-10 y1="calc(25%)" x2=35 y2="calc(25%)" stroke="{(currentScore>=15) ? "goldenrod" : "grey"}"></line>
    <line class="benchmark" x1=-10 y1="calc(0%)" x2=35 y2="calc(0%)" stroke="{(currentScore>=20) ? "goldenrod" : "grey"}"></line>
    <svg class="icon-thumbs-up" width="25px" stroke="grey" stroke-width="60px" fill="none" x=-5 y="calc(0% - 15px)" viewBox="0 0 1000 1000"><use href="#thumbs-up-path"></use></svg>
    {#if currentScore >= 10}
        <svg class="icon-thumbs-up" in:fade="{{duration: 500}}" width="25px" stroke="white" stroke-width="60px" fill="goldenrod" x=-5 y="calc(0% - 15px)" viewBox="0 0 1000 1000"><use href="#thumbs-up-path"></use></svg>
    {/if}
    
    <svg class="icon-thumbs-up" width="25px" stroke="grey" stroke-width="60px" fill="none" x=-5 y="calc(-25% - 15px)" viewBox="0 0 1000 1000"><use href="#thumbs-up-path"></use></svg>
    {#if currentScore >= 15}
        <svg class="icon-thumbs-up" in:fade="{{duration: 500}}" width="25px" stroke="white" stroke-width="60px" fill="goldenrod" x=-5 y="calc(-25% - 15px)" viewBox="0 0 1000 1000"><use href="#thumbs-up-path"></use></svg>
    {/if}
    
    <svg width="32px" stroke="grey" stroke-width="10px" fill="none" x=15 y="calc(-50% - 15px)" viewBox = "0 0 210 210"><polygon id="star" points="100,0,129.3893,59.5492,195.1056,69.0983,147.5528,115.4508,158.7785,180.9017,100,150,41.2215,180.9017,52.4472,115.4508,4.8943,69.0983,70.61073,59.54915"></polygon></svg>
    {#if currentScore === 20}
        <svg width="32px" in:fade="{{duration: 500}}" stroke="white" stroke-width="10px" fill="goldenrod" x=15 y="calc(-50% - 15px)" viewBox = "0 0 210 210"><polygon id="star" points="100,0,129.3893,59.5492,195.1056,69.0983,147.5528,115.4508,158.7785,180.9017,100,150,41.2215,180.9017,52.4472,115.4508,4.8943,69.0983,70.61073,59.54915"></polygon></svg>
    {/if}

    {#if currentScore === 20}
        {#each starArray as star,i}
            {#if i%2 === 0}
                <g class="star {(i % 4 === 0) ? "twinkle" : "" }" in:fade ="{{delay: 100*i, duration: 800}}" style="{(i % 4 === 0) ? `transform-origin:  ${-star.x + 80}px ${star.y - 30}px ` : ` ` }" >
                    <line x1={-star.x +80} y1={star.y - 30 + 2 + (i % 20) /20 *3} x2={-star.x +80} y2={star.y - 30 - 2 - (i % 20) /20 *3} stroke-width=1></line>
                    <line x1={-star.x + 80 + 2 + (i % 20) /20 *3} y1={star.y - 30} x2={-star.x + 80 - 2 - (i % 20) /20 *3} y2={star.y - 30} stroke-width=1 ></line>
                    <circle r="{(i % 20) /20 *2 + 1}" cx={-star.x + 80} cy={star.y - 30}></circle>    
                </g>
            {:else}
                <g class="star" in:scale ="{{delay: 100*i, duration: 1500}}">
                    <line x1={-star.x +80} y1={star.y - 30 + 2 + (i % 20) /20 *3} x2={-star.x +80} y2={star.y - 30 - 2 - (i % 20) /20 *3} stroke-width=1></line>
                    <line x1={-star.x + 80 + 2 + (i % 20) /20 *3} y1={star.y - 30} x2={-star.x + 80 - 2 - (i % 20) /20 *3} y2={star.y - 30} stroke-width=1 ></line>
                    <circle r="{(i % 20) /20 *2 + 1}" cx={-star.x + 80} cy={star.y - 30}></circle>    
                </g>
            {/if}
        
        {/each}
    {/if}
    
</svg>

<style>

    #progress-bar-svg{
        position: absolute;
        right: 22px;
        top: 42px;
        overflow: visible;
        width: 35px; 
        height: calc(100% - 100px);
    }
    @media only screen and (max-width: 600px){
        #progress-bar-svg{
            right: 8px;
            height: calc(100% - 85px);
        }
    }
    #progress-bar{
        fill: grey;
    }
    #progress{
        fill: goldenrod;
        filter: drop-shadow(0px 2px 4px white) drop-shadow(0px 3px 6px goldenrod);
    }
    .benchmark{
        stroke-width: 2px;
        stroke-dasharray: 4px;
    }
    
    .star circle {
        fill: url(#RadialGradient1);
        filter: drop-shadow(0px 0px 4px white) drop-shadow(0px 0px 6px goldenrod);
    }
    .star line{
        opacity: 0.5;
        stroke: gold;
        filter: drop-shadow(0px 0px 2px white) drop-shadow(0px 0px 4px goldenrod);
    }
    .twinkle{
        will-change: transform;
        animation: fade-frames 5000ms infinite linear;
    }


    @keyframes fade-frames {
      0% {
        transform: scale(0.5)
      }
      40% {
        transform: scale(0.5)
      }
      50% {
        transform: scale(1.5)
      }
      60% {
        transform: scale(0.5)
      }
      100% {
        transform: scale(0.5)
      }
    }  
    

    
</style>