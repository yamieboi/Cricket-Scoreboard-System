<div class="big-brother">
    <div style="font-size:2vh">
        <h1 style="font-family: Outfit; user-select:none; text-align:center; color:rgba(0, 0, 0, 0.6); ">
            <span style="color:rgba(0, 0, 0, 0.8);">Cricket Scoreboard</span> <br>
            Funtionality over beauty <br>
            Developed by <span style="color:rgba(0, 0, 0, 0.8)">Hasan M. Mahim</span>
        </h1>
    </div>

    <div class="match-setup">
        <div class="match-setup-title" style="font-family: Outfit; font-size: 4vh; padding-top: 1vh; font-weight: 500; user-select:none; text-align:center; color:rgba(255, 255, 255, 0.8);">Match Setup</div>
        <div class="team-names-field">
            <input class="team-name-input" type="text" minlength="4" maxlength="20">
            <input class="team-name-input" type="text" minlength="4" maxlength="20">
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

    .team-name-input{
        background-color: bisque;
        box-shadow: none;
    }
    input {border:0;outline:0;}
    input:focus {outline:none!important;}
/*     :global(body){
    } */
</style>