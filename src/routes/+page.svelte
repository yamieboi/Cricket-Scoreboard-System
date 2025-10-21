<script>
    import { fade } from 'svelte/transition';

    let matchSetupContainer;
    let MatchContainer;
    let teamNameContainer;
    let tossOversContainer;
    let tossDecisionContainer;
    let proceedButtonBox;
    let buttonProceed;
    let buttonSubmit;
    let matchTitle;
    let windowTitle;

    let buttonBatsmanModifier;
    let buttonRunsModifier;
    let buttonBowlerModifier;
    let teamOneTossButton;
    let teamTwoTossButton;
    let batButton;
    let bowlButton;
    let isBlurred = $state(false);


    let tossWinTeamDecision;
    let tossWinTeam;

    let battingTeamName = $state('');
    let bowlingTeamName = $state('');

    let teamOneName = $state('');
    let teamTwoName = $state('');
    let BatterInfoInput = $state('');
    let oversPerInnings = $state('1');
    let teamOnePlayers = $state([]);
    let teamTwoPlayers = $state([]);
    let tossResult = $state('Toss');

    let alreadyFlipped = false;
    let teamNamesEntered = false;

    let matchData = {}
    let teamOneBatting = {}
    let teamTwoBatting = {}

    let teamOneBowling = {}
    let teamTwoBowling = {}

    let testing = true

    function ProceedButton() {
        if (!teamNamesEntered && (teamOneName != '' && teamTwoName != '') && !(teamOneName == teamTwoName)) {
            teamNamesEntered = true;
            teamNameContainer.style.display = 'none';
            tossOversContainer.style = `       
                display: flex;
                flex-flow: column;
                place-content: center;
                align-items: flex-start;
                flex-wrap: wrap;
                align-content: center;
                justify-content: center;
            `;
        }

        if (alreadyFlipped && teamNamesEntered && (tossResult.value == 'Heads' || tossResult.value == 'Tails')) {
            tossOversContainer.style.display = 'none';
            tossDecisionContainer.style = `       
                display: flex;
                flex-flow: column;
                place-content: center;
                align-items: flex-start;
                flex-wrap: wrap;
                align-content: center;
                justify-content: center;
            `;

            buttonProceed.value = 'Start The Match!'
        };


        if (tossWinTeamDecision && tossWinTeam) {
            tossDecisionContainer.style.display = `none`;
            buttonProceed.style.display = 'none';
            windowTitle.innerHTML = "Match";
            matchSetupContainer.style.display = 'none';

            if (tossWinTeamDecision == 'bat') {
                battingTeamName = tossWinTeam;
                bowlingTeamName = (tossWinTeam == teamOneName) ? teamTwoName : teamOneName;
            } else {
                bowlingTeamName = tossWinTeam;
                battingTeamName = (tossWinTeam == teamOneName) ? teamTwoName : teamOneName;
            };

            MatchContainer.style = `
                height: 600px;
                width: 350px;
                color: rgb(255, 255, 255);
                background-color: rgba(3, 3, 3, 0.7);
                position: relative;
                display: flex;
                border-radius: 5px;
                flex-direction: column;
                flex-wrap: nowrap;
                align-content: center;
                justify-content: flex-start;
                align-items: center;
            `;

            console.log("batting team", battingTeamName, "bowling team", bowlingTeamName);
        };
    };


    function SubmitButton() {

    };

    function intiateToss() {
        if (!alreadyFlipped) {
            alreadyFlipped = true;
            tossResult.value = 'Dhoirjo Dhorun!';
            setTimeout(() => {
                var randomResult = Math.floor(Math.random() * 100);
                console.log(randomResult)
                if (randomResult <= 50) {
                    tossResult.value = 'Heads';
                } else {
                    tossResult.value = 'Tails';
                }
            }, 3000);
        };
    };

    function TossWinTeamSelect(p1, p2) {
        console.log(p1, p2);


        if (tossWinTeam != p1) {
            console.log('change colour');
            teamOneTossButton.style.backgroundColor = "#f0f0f0";
            teamTwoTossButton.style.backgroundColor = "#f0f0f0";
        };

        tossWinTeam = p1;
        if (p1 == teamOneName) {
            teamOneTossButton.style.backgroundColor = "#c9ffd4";
        } else {
            teamTwoTossButton.style.backgroundColor = "#c9ffd4";
        }
    };

    function TossWinTeamDecisionSelect(p1, p2) {
        console.log(p1, p2, tossWinTeamDecision, (tossWinTeamDecision != p1));

        if (tossWinTeamDecision != p1) {
            console.log('change colour');
            batButton.style.backgroundColor = "#f0f0f0";
            bowlButton.style.backgroundColor = "#f0f0f0";
        };

        tossWinTeamDecision = p1;
        if (p1 == "bat") {
            batButton.style.backgroundColor = "#c9ffd4";
        } else {
            bowlButton.style.backgroundColor = "#c9ffd4";
        }
        
        
    };

    function BatsmanButton(p1, p2) {
        isBlurred = true;

    };

    function BowlerButton(p1, p2) {
        isBlurred = true;
    };

    function RunsButton(p1, p2) {
        isBlurred = true;
    };
</script>

<div class="big-brother" transition:fade>
    <div style="font-size:2vh">
        <h1 style="font-family: Outfit; user-select:none; text-align:center; color:rgba(0, 0, 0, 0.6); ">
            <span style="color:rgba(0, 0, 0, 0.8);">Cricket Scoreboard</span> <br>
            Funtionality over beauty <br>
            Developed by <span style="color:rgba(0, 0, 0, 0.8)">Hasan M. Mahim</span>
        </h1>
    </div>

    <div class="match-setup" bind:this={matchSetupContainer}>
        <div class="match-setup-title" bind:this={windowTitle} style="font-family: Outfit; font-size: 2rem; padding-top: 10px; font-weight: 500; user-select:none; text-align:center; color:rgba(255, 255, 255, 0.8);">Match Setup</div>
        <div class="team-names-field" bind:this={teamNameContainer}>
            <div class="team-one-prompt-box">
                <input class="team-name-input" placeholder="First Team Name?" type="text" minlength="4" maxlength="12" bind:value={teamOneName}>
            </div>

            <div class="team-two-prompt-box">
                <input class="team-name-input" placeholder="Second Team Name?" type="text" minlength="4" maxlength="12" bind:value={teamTwoName}> 
            </div>

        </div>

        <div class="overs-toss-field" bind:this={tossOversContainer} style="display:none">
            <div class="overs-selection-box" style="margin-bottom: 5px;">
                <div class="overs-selection-prompt" style="font-family: Outfit; font-size: 1.2rem; padding-top: 0vh; font-weight: 500; user-select:none; color:rgba(255, 255, 255, 0.8);">How many overs per innings?</div>
                <input class="overs-input" type="number" min="1" bind:value={oversPerInnings}>
            </div>

            <div class="toss-box">
                <div class="toss-prompt" style="font-family: Outfit; font-size: 1.2em; padding-top: 0vh; font-weight: 500; user-select:none; color:rgba(255, 255, 255, 0.8);">Initiate Toss <br> (Let one team call.)</div>
                <input class="button-toss" type="button" value="Toss" bind:this={tossResult} on:click={intiateToss}>
            </div>
        </div>

        <div class="toss-decision-selection-field" bind:this={tossDecisionContainer} style="display:none">
            <div class="team-toss-win-box" style="margin-bottom: 10px; display: flex; flex-direction: column; align-items: center;">
                <div class="team-toss-win-prompt" style="font-family: Outfit; font-size: 1.2rem; padding-top: 0vh; font-weight: 500; user-select:none; color:rgba(255, 255, 255, 0.8);">Which team won the toss?</div>
                <input class="button-team-toss-win" type="button" value={teamOneName} bind:this={teamOneTossButton} on:click={() => TossWinTeamSelect(teamOneName)}>
                <input class="button-team-toss-win" type="button" value={teamTwoName} bind:this={teamTwoTossButton} on:click={() => TossWinTeamSelect(teamTwoName)}>
            </div>

            <div class="team-toss-decision-box" style="width: 100%; display: flex; flex-direction: row; justify-content: center; align-items: center; gap: 10px;">
                <!-- <div class="team-toss-decision-prompt" style="font-family: Outfit; font-size: 2vh; padding-top: 0vh; font-weight: 500; user-select:none; color:rgba(255, 255, 255, 0.8);">What have they decided to do? <br> (Bhai Bat Koiren Nah!)</div> -->
                <input class="button-team-toss-decision" type="button" value="Bat" bind:this={batButton} on:click={() => TossWinTeamDecisionSelect('bat')}>
                <input class="button-team-toss-decision" type="button" value="Bowl" bind:this={bowlButton}  on:click={() => TossWinTeamDecisionSelect('bowl')}>
            </div>
        </div>

        <div class="proceed-box" bind:this={proceedButtonBox}>
            <input class="button-proceed-box" type="button" value="Proceed" bind:this={buttonProceed} on:click={ProceedButton}>
        </div>
    </div>

    <div class="match-container" class:blurred={isBlurred} bind:this={MatchContainer} style="display:none">
        <div class="match-title" bind:this={matchTitle} style="font-family: Outfit; font-size: 2rem; padding-top: 10px; font-weight: 500; user-select:none; text-align:center; color:rgba(255, 255, 255, 0.8);">Match</div>
        <div class="batting-scorecard">
            <div class="batting-team-name">
                Now Batting {battingTeamName}
            </div>

            <div class="batting-team-scorebox">
                
            </div>
        </div>
        
        <div class="bowling-scorecard">
            <div class="bowling-team-name">
                Now Bowling {bowlingTeamName}
            </div>

            <div class="bowling-team-scorebox">
                
            </div>
        </div>


        <div class="info-input-container">
            <div class="batter-info-box" >
                <input class="batter-info-input" placeholder="Batsman name?" type="text" minlength="4" maxlength="10" bind:value={BatterInfoInput}> 
            </div>

            <input class="button-submit-box" type="button" value="Thik Aseh!" bind:this={buttonSubmit} on:click={SubmitButton}>
        </div>

        <input class="button-batsman-modifier" type="button" value="Add Batsman" bind:this={buttonBatsmanModifier} on:click={BatsmanButton}>
        <input class="button-bowler-modifier" type="button" value="Add Bowler" bind:this={buttonBowlerModifier} on:click={BowlerButton}>
        <input class="button-run-modifier" type="button" value="Add Runs" bind:this={buttonRunsModifier} on:click={RunsButton}>
    </div>
</div>


<style>
    .blurred{
        filter: blur(3px);
    }

    .big-brother{
        display: flex;
        align-content: center;
        column-gap: 1px;
        align-items: center;
        justify-items: center;
        gap: 1px 1px;
        flex-direction: column;
        flex-wrap: nowrap;
        justify-content: center;
        height: 100%;
        width: 100%;
    }

    .match-setup{
        height: 300px;
        width: 350px;
        color: rgb(255, 255, 255);
        background-color: rgba(3, 3, 3, 0.7);
        position: relative;
        display: flex;
        border-radius: 5px;
        flex-direction: column;
        flex-wrap: nowrap;
        align-content: center;
        justify-content: flex-start;
        align-items: center;
    }

    .team-one-prompt-box{
/*         margin-top: -190px;
        position: absolute; */
        margin-top: 10px;

    }

    .team-two-prompt-box{
/*         margin-top: -1vh;
        position: absolute; */
                margin-top: 10px;
    }

    .team-names-field{
        display: flex;
        flex-direction: column;
        flex-wrap: nowrap;
        justify-content: center;
        align-items: center;
    }

    .proceed-box{
        margin-top: 230px;
        position: absolute;
    }

    .button-batsman-modifier{
        height: 30px;
        width: 150px;
        color: rgba(0, 0, 0, 0.8);
        font-family: Outfit;
        font-size: 1.2rem;
        text-align: center;
        font-weight: 600;
        border-radius: 5px;
        border: 0;
        /* padding-bottom: 10px; */
        outline: 0;
        top: 460px;
        position: absolute;
    }

    .button-batsman-modifier:active{
        scale: 0.98;
    }

    .button-bowler-modifier{
        height: 30px;
        width: 150px;
        color: rgba(0, 0, 0, 0.8);
        font-family: Outfit;
        font-size: 1.2rem;
        text-align: center;
        font-weight: 600;
        border-radius: 5px;
        border: 0;
        /* padding-bottom: 10px; */
        outline: 0;
        top: 500px;
        position: absolute;
    }

    .button-bowler-modifier:active{
        scale: 0.98;
    }

    .button-run-modifier{
        height: 30px;
        width: 150px;
        color: rgba(0, 0, 0, 0.8);
        font-family: Outfit;
        font-size: 1.2rem;
        text-align: center;
        font-weight: 600;
        border-radius: 5px;
        border: 0;
        /* padding-bottom: 10px; */
        outline: 0;
        top: 540px;
        position: absolute;
    }

    .button-run-modifier:active{
        scale: 0.98;
    }
    
    .button-proceed-box{
        height: 50px;
        width: 300px;
        color:rgba(0, 0, 0, 0.8);
        font-family: Outfit; 
        font-size: 1.6rem;
        font-weight: 600;
        border-radius: 5px;
        border:0;
        outline:0;
    }

    .button-proceed-box:active{
        scale: 0.98;
    }


    .button-team-toss-win:active{
        scale: 0.98;
    }

    .button-team-toss-decision:active{
        scale: 0.98;
    }

    .batting-team-name{
        font-family: Outfit;
        font-size: 1.2rem;
        padding-top: 10px;
        font-weight: 500;
        user-select: none;
        text-align: center;
        color: rgb(255 255 255);
        margin-bottom: 5px;
    }

    .batting-team-scorebox{
        display: flex;
        flex-direction: column;
        flex-wrap: nowrap;
        justify-content: center;
        align-items: center;
        height: 150px;
        max-height: 150px;
        width: 320px;
        border-radius: 5px;
        background-color: #ffffff4d;
    }

    .bowling-team-name{
        font-family: Outfit;
        font-size: 1.2rem;
        padding-top: 10px;
        font-weight: 500;
        user-select: none;
        text-align: center;
        color: rgb(255 255 255);
        margin-bottom: 5px;
    }

    .bowling-team-scorebox{
        display: flex;
        flex-direction: column;
        flex-wrap: nowrap;
        justify-content: center;
        align-items: center;
        height: 150px;
        max-height: 150px;
        width: 320px;
        border-radius: 5px;
        background-color: #ffffff4d;
    }
/*     .overs-toss-field{
        display: flex;
        flex-direction: column;
        flex-wrap: nowrap;
        align-content: center;
        justify-content: center;
        align-items: center;
        margin-top: 15vh;
    } */


    .button-toss{
        height: 40px;
        width: 300px;
        color:rgba(0, 0, 0, 0.8);
        font-family: Outfit; 
        font-size: 1.8rem; 
        font-weight: 600;
        border-radius: 5px;
        border:0;
        outline:0;
        margin-top: 5px;
    }

    .button-toss:active{
        scale: 0.98; 
    }


    .team-name-input{
        height: 50px;
        width: 300px;
        background-color: rgb(240 240 240);
        box-shadow: none;
        font-family: Outfit; 
        font-size: 1.5rem;
        font-weight: 500;
        border-radius: 5px;
        color:rgba(0, 0, 0, 0.925);
        user-select:none;
        text-align: center;
    }

    .overs-input{
        height: 30px;
        width: 50px;
        background-color: rgb(240 240 240);
        box-shadow: none;
        font-family: Outfit;
        font-size: 1.2rem;
        font-weight: 600;
        border-radius: 5px;
        color: rgba(0, 0, 0, 0.925);
        user-select: none;
        text-align: center;
        margin-top: 5px;
    }

    .button-team-toss-win{
        height: 40px;
        width: 300px;
        color:rgba(0, 0, 0, 0.8);
        font-family: Outfit; 
        font-size: 1.6rem; 
        font-weight: 600;
        border-radius: 5px;
        border:0;
        outline:0;
        margin-top: 5px;
    }

    .button-team-toss-decision{
        height: 30px;
        width: 100px;
        color: rgba(0, 0, 0, 0.8);
        font-family: Outfit;
        font-size: 1.4rem;
        font-weight: 600;
        border-radius: 5px;
        border: 0;
        outline: 0;
        margin-top: 5px;
    }

    input {border:0;outline:0;}
    input:focus {outline:none!important;}
/*     :global(body){
    } */
</style>