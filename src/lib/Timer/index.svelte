<svg>
    <circle r="18" cx="42" cy="42" style={gameTimer ? "animation: countdown 120s linear infinite forwards" : ""}></circle>
    <text x="42" y="32">2</text>
    <text x="42" y="52">min</text>
</svg>

<script>
    export let gameTimer;
</script>

<style>
    svg{
        width: 85px;
        height: 85px;
        position: absolute;
        top: 8px;
        right: 8px;
    }
    @media only screen and (max-width: 600px){
        svg{
            right: -5px;
        }
    }
    svg circle {
        stroke-dasharray: 113px;
        stroke-dashoffset: 0px;
        stroke-width: 36px;
        stroke: goldenrod;
        fill: none;
    }
    @keyframes -global-countdown {
        from {
            stroke-dashoffset: 0px;
        }
        to {
            stroke-dashoffset: 113px;
        }
    }
    svg text{
        text-anchor: middle;
        stroke: none;
        fill: #2c2f33;
        font-family: arial;
    }
</style>