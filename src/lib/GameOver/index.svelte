<script>
    import { onMount } from 'svelte';
    export let currentScore, numErrors, gameOver, gameTimer;
    onMount(()=>{
        document.querySelector("#game-over-message").style.opacity = 1;
        let buttons = document.querySelectorAll("#button-wrapper button");
        for(let i=0; i<buttons.length; i++){
            buttons[i].disabled = true; 
        }
    });
    setTimeout(()=>{  
        document.querySelector("#game-over-message").style.opacity = 0;
        document.querySelector("#num-correct").style.opacity = 1;
        document.querySelector("#num-errors").style.opacity = 1;
    },1500);
    setTimeout(()=>{
        document.querySelector("#success-message").style.opacity = 1;

        console.log('the game is over');
        let gameSettings = document.querySelector("#trig-functions").value + "; " + document.querySelector("#angle-units").value.substr(0,15) + "; " + document.querySelector("#angle-quadrants").value;
        window.parent.postMessage({"message":"the game is over","correct":currentScore,"errors":numErrors,"settings":gameSettings},"https:/10minutequiz.com/")

    },3000);
    setTimeout(()=>{
        document.querySelector("#play-again").style.opacity = 1;
        document.querySelector("#play-again").disabled = false;
    },4500);

    let successMessage;
    if(currentScore < 8){successMessage = "Keep practising!"}
    else if(currentScore < 10){successMessage = "Almost there!"}
    else if(currentScore < 12){successMessage = "Pretty good!"}
    else if(currentScore < 15){successMessage = "Well done!"}
    else if(currentScore < 18){successMessage = "Great!"}
    else if(currentScore < 20){successMessage = "Excellent!"}
    else{successMessage = "Exact Value STAR!"}

</script>

<div id="game-over-screen">
    <div>
        <p id="game-over-message">
            {currentScore < 20 ? "Time is up!" : "You did it!"}
        </p>
        <ul>
            <li id="num-correct">{currentScore} correct</li>
            <li id="num-errors">{numErrors} {numErrors === 1 ? "error" : "errors"}</li>
        </ul>
        <p id="success-message">
            {successMessage}
        </p>
        <p>
            <button id="play-again" disabled="true" on:click="{()=>{
                currentScore = 0; 
                numErrors = 0; 
                gameOver = false;
                gameTimer = setTimeout(()=>{
                    gameOver = true;
                },120000)
            }}">Try again</button>
        </p>
        
    </div>
</div>

<style>
    #game-over-screen{
        width: 90%;
        height: 90vh;
        display: flex;
		flex-direction: column;
		justify-content: center;
		align-items: center;
        color: white;
        z-index: 1;
        position: absolute;
        top: calc(50% - 45vh);
        left: calc(50% - 45%);
    }
    #game-over-screen p{
        text-align: center;
    }
    #game-over-screen div{
        max-width: 500px;
        font-size: 36px;
    }
    ul{
        line-height: 2;
    }
    #game-over-message, #num-correct, #num-errors, #success-message{
        opacity: 0;
        transition: opacity 2s ease-out;
    }
    #success-message{
        text-shadow: 0 0 10px white, 0 0 20px goldenrod, 0 0 30px goldenrod;
        }
    
    button{
        font-size: 36px;
        margin: 2vw;
        padding: 20px;
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
        font-family: KaTeX_Main;    
        opacity: 0;
        transition: opacity 2s ease-out; 
    }
</style>