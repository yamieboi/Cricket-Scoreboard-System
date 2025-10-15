<script>
    import { fade } from 'svelte/transition';

    let matchSetupContainer;
    let teamNameContainer;
    let tossOversContainer;
    let proceedButtonBox;

    let teamOneName = $state('');
    let teamTwoName =  $state('');
    let oversPerInnings = $state('1');
    let teamOnePlayers = $state([]);
    let teamTwoPlayers = $state([]);
    let tossResult = $state('Toss');

    let alreadyFlipped = false;
    let teamNamesEntered = false;

    function ProceedButton() {
        if (!teamNamesEntered && (teamOneName != '' && teamTwoName != '')) {
            teamNamesEntered = true;
            teamNameContainer.style.display = 'none';
            tossOversContainer.style = `       
                display: flex;
                flex-flow: column;
                place-content: center;
                align-items: flex-start;
                margin-top: 1vh;
                flex-wrap: wrap;
                align-content: center;
                justify-content: center;
            `;
        }

        if (alreadyFlipped && teamNamesEntered) {
            tossOversContainer.style.display = 'none';
            proceedButtonBox.style.display = 'none';
/*             matchSetupContainer.style.height = '100vh';
            matchSetupContainer.style.width = '70vw';  */
        };
    };

    function intiateToss() {
        if (!alreadyFlipped) {
            alreadyFlipped = true;
            tossResult.value = 'Dhoirjo Dhorun!';
            let randomResult = Math.floor(Math.random() * 100);
            setTimeout(() => {
                if (randomResult <= 50) {
                    tossResult.value = 'Heads';
                } else {
                    tossResult.value = 'Tails';
                }
            }, 3000);
        };
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
        <div class="match-setup-title" style="font-family: Outfit; font-size: 4vh; padding-top: 2vh; font-weight: 500; user-select:none; text-align:center; color:rgba(255, 255, 255, 0.8);">Match Setup</div>
        <div class="team-names-field" bind:this={teamNameContainer}>
            <div class="team-one-prompt-box">
                <!-- <div class="team-name-ask" style="font-family: Outfit; font-size: 2vh; padding-top: 0vh; font-weight: 500; user-select:none; color:rgba(255, 255, 255, 0.8);">First Team Name?</div> -->
                <input class="team-name-input" placeholder="First Team Name?" type="text" minlength="4" maxlength="20" bind:value={teamOneName}>
            </div>

            <div class="team-two-prompt-box">
                <!-- <div class="team-name-ask" style="font-family: Outfit; font-size: 2vh; padding-top: 0vh; font-weight: 500; user-select:none; color:rgba(255, 255, 255, 0.8);">Second Team Name?</div> -->
                <input class="team-name-input" placeholder="Second Team Name?" type="text" minlength="4" maxlength="20" bind:value={teamTwoName}> 
            </div>

<!--             <div class="proceed-box">
                <input class="button-proceed-box" type="button" value="Proceed" on:click={proceedToMatchSettings}>
            </div> -->

        </div>

        <div class="overs-toss-field" bind:this={tossOversContainer} style="display:none">
            <div class="overs-selection-box" style="margin-bottom: 1vh;">
                <div class="overs-selection-prompt" style="font-family: Outfit; font-size: 2vh; padding-top: 0vh; font-weight: 500; user-select:none; color:rgba(255, 255, 255, 0.8);">How many overs per innings?</div>
                <input class="overs-input" type="number" min="1" bind:value={oversPerInnings}>
            </div>

            <div class="toss-box">
                <div class="toss-prompt" style="font-family: Outfit; font-size: 2vh; padding-top: 0vh; font-weight: 500; user-select:none; color:rgba(255, 255, 255, 0.8);">Initiate Toss <br> (Let one team call.)</div>
                <input class="button-toss" type="button" value="Toss" bind:this={tossResult} on:click={intiateToss}>
            </div>
        </div>

        <div class="toss-decision-selection-field" bind:this={tossOversContainer} style="display:none">
            <div class="team-toss-win-box" style="margin-bottom: 1vh;">
                <div class="team-toss-win-prompt" style="font-family: Outfit; font-size: 2vh; padding-top: 0vh; font-weight: 500; user-select:none; color:rgba(255, 255, 255, 0.8);">How many overs per innings?</div>
                <input class="overs-input" type="number" min="1" bind:value={oversPerInnings}>
            </div>

            <div class="toss-box">
                <div class="toss-prompt" style="font-family: Outfit; font-size: 2vh; padding-top: 0vh; font-weight: 500; user-select:none; color:rgba(255, 255, 255, 0.8);">Initiate Toss <br> (Let one team call.)</div>
                <input class="button-toss" type="button" value="Toss" bind:this={tossResult} on:click={intiateToss}>
            </div>
        </div>

        <div class="proceed-box" bind:this={proceedButtonBox}>
            <input class="button-proceed-box" type="button" value="Proceed" on:click={ProceedButton}>
        </div>

    </div>
</div>


<style>
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
    }
    .match-setup{
        height: 40vh;
        width: 35vw;
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
        margin-top: -20vh;
        position: absolute;
    }

    .team-two-prompt-box{
        margin-top: -1vh;
        position: absolute;
    }

    .team-names-field{
        margin-top: 15vh;
        display: flex;
        flex-direction: column;
        flex-wrap: nowrap;
        justify-content: center;
        align-items: center;
    }

    .proceed-box{
        margin-top: 30vh;
        position: absolute;
    }
    
    .button-proceed-box{
        height: 6vh;
        width: 25vw;
        color:rgba(0, 0, 0, 0.8);
        font-family: Outfit; 
        font-size: 3vh; 
        font-weight: 600;
        border-radius: 5px;
        border:0;
        outline:0;
    }

    .button-proceed-box:active{
        scale: 0.98;
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
        height: 6vh;
        width: 25vw;
        color:rgba(0, 0, 0, 0.8);
        font-family: Outfit; 
        font-size: 3vh; 
        font-weight: 600;
        border-radius: 5px;
        border:0;
        outline:0;
        margin-top: 0.5vh;
    }

    .button-toss:active{
        scale: 0.98; 
    }


    .team-name-input{
        height: 6vh;
        width: 25vw;
        background-color: rgb(255, 255, 255);
        box-shadow: none;
        font-family: Outfit; 
        font-size: 3vh; 
        font-weight: 500;
        border-radius: 5px;
        color:rgba(0, 0, 0, 0.925);
        user-select:none;
        text-align: center;
    }

    .overs-input{
        height: 3vh;
        width: 3vw;
        background-color: rgb(255, 255, 255);
        box-shadow: none;
        font-family: Outfit;
        font-size: 1.2rem;
        font-weight: 600;
        border-radius: 5px;
        color: rgba(0, 0, 0, 0.925);
        user-select: none;
        text-align: center;
        margin-top: 0.5vh;
    }

    input {border:0;outline:0;}
    input:focus {outline:none!important;}
/*     :global(body){
    } */
</style>