<script>
    import { fade } from 'svelte/transition';

    let teamNameContainer;
    let matchSetupContainer;
    let teamOneName = $state('');
    let teamTwoName =  $state('');
    let teamOnePlayers = $state([]);
    let teamTwoPlayers = $state([]);

    function proceedToMatchSettings() {
        teamNameContainer.style.display = 'none';
        matchSetupContainer.style.height = '100vh';
        matchSetupContainer.style.width = '70vw';
        /* teamNameContainer.fade(); */
        console.log(TeamOneName, TeamTwoName)
        console.log()  
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
        <div class="team-names-field" bind:this={teamNameContainer}  out:fade>
            <div class="team-one-prompt-box">
                <!-- <div class="team-name-ask" style="font-family: Outfit; font-size: 2vh; padding-top: 0vh; font-weight: 500; user-select:none; color:rgba(255, 255, 255, 0.8);">First Team Name?</div> -->
                <input class="team-name-input" placeholder="First Team Name?" type="text" minlength="4" maxlength="20" bind:value={TeamOneName}>
            </div>

            <div class="team-two-prompt-box">
                <!-- <div class="team-name-ask" style="font-family: Outfit; font-size: 2vh; padding-top: 0vh; font-weight: 500; user-select:none; color:rgba(255, 255, 255, 0.8);">Second Team Name?</div> -->
                <input class="team-name-input" placeholder="Second Team Name?" type="text" minlength="4" maxlength="20" bind:value={TeamTwoName}> 
            </div>

            <div class="proceed-box">
                <input class="button-proceed-box" type="button" value="Proceed" on:click={proceedToMatchSettings}>
            </div>

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
        height:40vh;
        width:35vw;
        color: rgb(255, 255, 255);
        background-color: rgba(3, 3, 3, 0.7);
        position: relative;
        border-radius: 5px;
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
        margin-top: 3vh;
        display: flex;
        flex-direction: column;
        flex-wrap: nowrap;
        justify-content: center;
        align-items: center;
    }

    .proceed-box{
        margin-top: 20vh;
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
    input {border:0;outline:0;}
    input:focus {outline:none!important;}
/*     :global(body){
    } */
</style>