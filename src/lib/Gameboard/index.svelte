<div id="game-board">
    <UnitCircle {currentAngle} {showAngle} {showLength}/>
    <ProgressBar {currentScore}/>
    <ToolBar bind:toolBarOpen bind:trigFunctions bind:angleTypes bind:angleQuadrants bind:settingChanged bind:gameTimer/>
    {#if gameOver}
        <GameOver bind:currentScore bind:numErrors bind:gameOver/>
    {:else}
        <Question {currentQuestion}/>
        <MCOptions {correctClickFunc} {wrongClickFunc} {theOptions}/>    
        <Timer {gameTimer}/>
    {/if}
</div>

<script>
	import Timer from '$lib/Timer/index.svelte';
    import Question from '$lib/Question/index.svelte';
    import UnitCircle from '$lib/UnitCircle/index.svelte';
    import MCOptions from '$lib/MCOptions/index.svelte';
    import ProgressBar from '$lib/ProgressBar/index.svelte';
    import GameOver from '$lib/GameOver/index.svelte';
    import ToolBar from '$lib/ToolBar/index.svelte';
    
    // initialise variables 
    const Q1Angles = ["0","30","45","60","90",];
    const Q2Angles = ["120","135","150","180"];
    const Q34Angles = ["210","225","240","270","300","315","330","360","390","405","420","450","480","495","510","540"];
    const negativeAngles = ["-30","-45","-60","-90","-120","-135","-150","-180","-210","-225","-240","-270","-300","-315","-330"];
    let gameOver = false, gameTimer = 0, currentScore = 18, numErrors = 0, toolBarOpen = false;
    let correctAns, angleType, showAngle = false, showLength = "", wrongAgain = false, settingChanged = false;;
    let trigFunctions = ["sin","cos","tan"], angleTypes = ["degrees","radians","radians"], angleQuadrants = ["Q1","Q2","Q3","Q4"];
    let possibleAngles = Q1Angles.concat(Q2Angles).concat(Q34Angles);      
    let currentQuestion, currentFunction = "sin", currentAngle = "30", exactAns = [], distractors = [], theOptions;
    
    // set first question (always the same)
    currentQuestion = ["sin","pi","6"];
    exactAns = [{"text":["frac","1","2"], "correct":1}];
    distractors = [{"text":["frac","3","2"], "correct":0}, {"text":["2"], "correct":0}, {"text":["sqrt","5","2"], "correct":0}];
    theOptions = exactAns.concat(distractors);
    theOptions.sort();
    
    const arraysMatch = function(r,n){if(r.length!==n.length)return!1;for(var t=0;t<r.length;t++)if(r[t]!==n[t])return!1;return!0};
    const exactVal = {
        "0.5":["frac","1","2"],
        "0.577":["sqrt","3","3"],
        "0.707":["sqrt","2","2"],
        "0.866":["sqrt","3","2"],
        "1.732":["sqrt","3",""],
       }

    let correctClickFunc = (e)=>{
        if(gameTimer === 0){
            gameTimer = 
            setTimeout(()=>{
                gameOver = true;
            },120000)
        }
        toolBarOpen = false;
        currentScore += 1;
        if(currentScore === 20){
            gameOver = true;
            window.clearTimeout(gameTimer);
        }
        currentQuestion = [];
        let buttons = document.querySelectorAll("#button-wrapper button");
        for(let i=0; i<buttons.length; i++){
            buttons[i].disabled = true; 
        }
        e.currentTarget.disabled = false;
        e.currentTarget.style.boxShadow = "0 0 10px white, 0px 0px 20px gold, 0px 0px 40px goldenrod";
        //e.currentTarget.style.boxShadow = "0 0 3px white, 0px 0px 6px goldenrod";
        let questionText = document.querySelector("#question .ML__mathlive");
        questionText.style.textShadow = "0 0 10px white, 0 0 20px goldenrod, 0 0 30px goldenrod";
        showAngle = true;
        showLength = currentFunction;
            
        setTimeout((e)=>{
            currentAngle = possibleAngles[Math.floor(Math.random()*possibleAngles.length)];
            angleType = angleTypes[Math.floor(Math.random()*angleTypes.length)];
            currentFunction = trigFunctions[Math.floor(Math.random()*trigFunctions.length)];
        
            if(angleType === "degrees"){
                currentQuestion = [currentFunction,currentAngle,""]
            }
            else if(currentAngle % 360 === 0){
                currentQuestion = [currentFunction, "0", ""]
            }
            else if(currentAngle % 180 === 0){
                currentQuestion = [currentFunction, Math.round(currentAngle / 180) + "pi", ""]
            }
            else if(currentAngle % 90 === 0){
                currentQuestion = [currentFunction, Math.round(currentAngle / 90) + "pi", "2"]
            }
            else if(currentAngle % 60 === 0){
                currentQuestion = [currentFunction, Math.round(currentAngle / 60) + "pi","3"]
            }
            else if(currentAngle % 45 === 0){
                currentQuestion = [currentFunction, Math.round(currentAngle / 45) + "pi","4"]
            }
            else if(currentAngle % 30 === 0){
                currentQuestion = [currentFunction, Math.round(currentAngle / 30) + "pi","6"]
            }
            correctAns = Math.round(1000*Math[currentFunction](currentAngle * 3.14159 / 180))/1000;
            
            let possibleDistractors = [];
            if(Math.abs(correctAns) > 1000){
                exactAns = [{"text":["undef"] , "correct":1}];
                possibleDistractors.push(["0"], ["1"], ["-1"], ["sqrt","2","2"], ["sqrt","3","2"]);
            }
            else if([0,-0].includes(correctAns)){
                exactAns = [{"text":["0"] , "correct":1}];
                possibleDistractors.push(["undef"], ["1"], ["-1"], ["sqrt","2","2"], ["sqrt","2","2","neg"]);
            }
            else if([1,-1].includes(correctAns)){
                exactAns = [{"text":[correctAns.toString()] , "correct":1}];
                possibleDistractors.push(["undef"], ["0"], ["sqrt","2","2"], ["sqrt","2","2","neg"], ["sqrt","3",""],);
            }
            else if(correctAns > 0){
                exactAns = [{"text": exactVal[correctAns.toString().substr(0,5)] , "correct":1}];
                for (const prop in exactVal) {
                    if(!arraysMatch(exactVal[prop] , exactVal[correctAns.toString().substr(0,5)] )){
                        possibleDistractors.push(exactVal[prop]);
                        if(!Q1Angles.includes(currentAngle)){
                            possibleDistractors.push(exactVal[prop].concat(["neg"]))
                        }
                    }
                }             
            }
            else if(correctAns < 0){
                exactAns = [{"text": exactVal[correctAns.toString().substr(1,6)].concat(["neg"]) , "correct":1}];
                for (const prop in exactVal) {
                    if(!arraysMatch(exactVal[prop].concat(["neg"]), exactVal[correctAns.toString().substr(1,6)].concat(["neg"]))){
                        possibleDistractors.push(exactVal[prop].concat(["neg"]));
                        possibleDistractors.push(exactVal[prop]);
                    }
                }
            }
            
            let len = possibleDistractors.length;
            let r = Math.floor(Math.random()*len);   
            distractors = [{"text":possibleDistractors[r], "correct":0}, {"text":possibleDistractors[(r+1)%len], "correct":0}, {"text":possibleDistractors[(r+2)%len], "correct":0}];
            theOptions = exactAns.concat(distractors);
            theOptions.sort((a, b) => (a.text > b.text) ? 1 : -1);
            for(let i=0; i<buttons.length; i++){
                buttons[i].disabled = false; 
                buttons[i].style.boxShadow = "0px 0px 0px goldenrod"; 
            }
            questionText.style.textShadow = ""
            
            showAngle = false;
            wrongAgain = false;
            showLength = "";
        },1000);
    }

    let wrongClickFunc = (e)=>{
        toolBarOpen = false;
        numErrors += 1;
        showAngle = true;
        if(wrongAgain){
            showLength = currentFunction;
        }
        wrongAgain = true;
        currentScore = Math.max(0, currentScore - 1);
        e.currentTarget.disabled = true;
    }

    $:if(gameOver){
        let buttons = document.querySelectorAll("#button-wrapper button");
        for(let i=0; i<buttons.length; i++){
                buttons[i].disabled = true; 
            }
    }

    // if setting changes, redefine initial question and restart game     NEED TODO  - reset timer
    $:if(settingChanged){
        console.log('setting changed (restart game)')
        gameOver = true;
        window.clearTimeout(gameTimer);
        gameTimer = 0;
        currentScore = 0; 
        numErrors = 0;
        if(angleTypes.includes("radians")){
            currentAngle = "30", currentFunction = "sin", currentQuestion = ["sin","pi","6"];
            exactAns = [{"text":["frac","1","2"], "correct":1}];
            distractors = [{"text":["frac","3","2"], "correct":0}, {"text":["2"], "correct":0}, {"text":["sqrt","5","2"], "correct":0}];
        }
        else{
            currentAngle = "30", currentFunction = "sin", currentQuestion = ["sin","30"];
            exactAns = [{"text":["frac","1","2"], "correct":1}];
            distractors = [{"text":["frac","3","2"], "correct":0}, {"text":["2"], "correct":0}, {"text":["sqrt","5","2"], "correct":0}];
        } 
        theOptions = exactAns.concat(distractors);
        theOptions.sort();
        possibleAngles = Q1Angles;
        if(angleQuadrants.includes("Q2")){
            possibleAngles = possibleAngles.concat(Q2Angles); 
        }
        if(angleQuadrants.includes("Q3")){
            possibleAngles = possibleAngles.concat(Q34Angles); 
        }
        if(angleQuadrants.includes("neg")){
            possibleAngles = possibleAngles.concat(negativeAngles); 
        }
        gameOver = false;
        settingChanged = false;
        let buttons = document.querySelectorAll("#button-wrapper button");
        for(let i=0; i<buttons.length; i++){
            buttons[i].disabled = false; 
        }
        let sinArrow = document.querySelector("#sinArrow");
        if(sinArrow){sinArrow.style.opacity = 0;}
        let cosArrow = document.querySelector("#cosArrow");
        if(cosArrow){cosArrow.style.opacity = 0;}
        let tanArrow = document.querySelector("#tanArrow");
        if(tanArrow){tanArrow.style.opacity = 0;}
    }
    
</script>

<style>
    #game-board{
        width: 100vw;
        height: 100vh;
        border: 5px solid rgb(95, 7, 7);
        border-radius: 20px;
        background:#2c2f33;
        display: flex;
		flex-direction: column;
		justify-content: center;
		align-items: center;
        position: relative;
        padding-bottom: 30vh; /*space for the answer buttons*/
        box-sizing: border-box;
    }
    @media only screen and (max-width: 600px){
        #game-board{
            padding-bottom: 40vh;
        }
    }
</style>