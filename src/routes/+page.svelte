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
    let buttonBallsModifier;
    let buttonBowlerModifier;
    let teamOneTossButton;
    let teamTwoTossButton;
    let infoInputContainer;
    let ballInputContainer;
    let batterInfoBox;
    let BattingTeamScorebox;
    let bowlerInfoBox;
    let batButton;
    let bowlButton;
    let isBlurred = $state(false);

    
    let ZeroRunsButton;
    let OneRunsButton;
    let TwoRunsButton;
    let ThreeRunsButton;
    let FourRunsButton;
    let FiveRunsButton;
    let SixRunsButton;
    let WideRunsButton;
    let NoBallButton;
    let ByeRunsButton;
    let LegByeRunsButton;
    let overallScoreCard;
    let WicketsButton;

    let firstInningsEnd = false;

    let tossWinTeamDecision;
    let tossWinTeam;

    let battingTeamName = $state('');
    let bowlingTeamName = $state('');

    let teamOneName = $state('');
    let teamTwoName = $state('');
    let BatterInfoInput = $state('');
    let BowlerInfoInput = $state('');
    let oversPerInnings = $state('1');
    let teamOnePlayers = $state([]);
    let teamTwoPlayers = $state([]);
    let tossResult = $state('Toss');

    let alreadyFlipped = false;
    let teamNamesEntered = false;

    let matchData = $state({})
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
                height: 650px;
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
                user-select: none;
                align-items: center;
            `;

            overallScoreCard.style = `
                height: 50px;
                width: 350px;
                color: rgb(255, 255, 255);
                background-color: rgba(3, 3, 3, 0.7);
                position: relative;
                display: flex;
                border-radius: 5px;
                flex-flow: column;
                place-content: center flex-start;
                user-select: none;
                align-items: center;
                margin-bottom: 5px;
                flex-wrap: nowrap;
                flex-direction: row;
                align-content: center;
                justify-content: flex-start;
                
            `;

            matchData.battingTeamName = {
                isBatting: true,
                isBowling: false,
                battingData: {},
                bowlingData: {},
                availableBowlers: [],
                availableBatters: [],
                totalBatters: 0,
                totalBowlers: 0
            };

            matchData.bowlingTeamName = {
                isBatting: false,
                isBowling: true,
                battingData: {},
                bowlingData: {},
                availableBowlers: [],
                availableBatters: [],
                totalBatters: 0,
                totalBowlers: 0
            }

            matchData.currentOver = [];
            matchData.selectedOptionsForBall = [];
            matchData.ballCount = 0;
            matchData.runCount = 0;
            matchData.totalWickets = 0;
            matchData.currentOverNumber = 0;
            matchData.oversData = {};
            matchData.maxBallCount = (oversPerInnings * 6);
            matchData.oversCountFlt = 0;
            matchData.oversCount = 0;
            matchData.selectedExclusiveOptions = false;

            console.log("batting team", battingTeamName, "bowling team", bowlingTeamName);
        };
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

    let openedInputBox = false;

    function BatsmanButton(p1, p2) {
        if (openedInputBox) {
            return;
        }

        isBlurred = true;
        openedInputBox = 'batsmanName';
        infoInputContainer.style.display = "flex";
        batterInfoBox.style.display = 'block';

    };

    function BowlerButton(p1, p2) {
        if (openedInputBox) {
            return;
        }

        isBlurred = true;
        openedInputBox = 'bowlerName';
        infoInputContainer.style.display = "flex";
        bowlerInfoBox.style.display = 'block';
    };

    function AddBallButton(p1, p2) {
        if (openedInputBox) {
            return;
        }

        if ((matchData.bowlingTeamName.previousBowler) && (matchData.bowlingTeamName.activeBowler == matchData.bowlingTeamName.previousBowler)) {
            return;
        };

        if ((matchData.bowlingTeamName.bowlingData.length == 0) || (matchData.battingTeamName.battingData.length == 0)) {
            return;
        };

        isBlurred = true;
        openedInputBox = 'Runs';
        ballInputContainer.style.display = "flex";
    };

    function CreateActivePlayer(p1) {
        if (p1 == 'batting') {
            for (let name in matchData.battingTeamName.battingData) {
                if ((matchData.battingTeamName.activeBatsman == null) || (matchData.battingTeamName.activeBatsman == false)) {
                    let newName = matchData.battingTeamName.battingData[name].name + '*'

                    matchData.battingTeamName.activeBatsman = name;
                    matchData.battingTeamName.battingData[name].name = newName;
                    matchData.battingTeamName.activeBatsmanIndex = matchData.battingTeamName.battingData[name].index;
                    matchData.battingTeamName.activeBatsmanArrayIndex = matchData.battingTeamName.battingData[name].arrayIndex;
                }
            }
        };

        if (p1 == 'bowling') {
            for (let name in matchData.bowlingTeamName.bowlingData) {
                if ((matchData.bowlingTeamName.activeBowler == null) || (matchData.bowlingTeamName.activeBowler == false)) {
                    let newName = matchData.bowlingTeamName.bowlingData[name].name + '*'

                    matchData.bowlingTeamName.activeBowler = name;
                    matchData.bowlingTeamName.bowlingData[name].name = newName;
                    matchData.bowlingTeamName.activeBowlerIndex = matchData.bowlingTeamName.bowlingData[name].index;
                    matchData.bowlingTeamName.activeBowlerArrayIndex = matchData.bowlingTeamName.bowlingData[name].arrayIndex;
                }

/*                 if ((matchData.bowlingTeamName.activeBowler) && (matchData.bowlingTeamName.bowlingData.length > 1)) {
                    if (!(matchData.bowlingTeamName.activeBowler == name)) {
                        let newName = matchData.bowlingTeamName.bowlingData[name].name + '*'

                        matchData.bowlingTeamName.previousBowler = matchData.bowlingTeamName.activeBowler;
                        matchData.bowlingTeamName.activeBowler = name;
                        matchData.bowlingTeamName.bowlingData[name].name = newName
                    };
                } */
            }
        };
    };

    function SubmitButton() {
        isBlurred = false;
        

        infoInputContainer.style.display = "none";
        batterInfoBox.style.display = 'none';
        bowlerInfoBox.style.display = 'none';

        if ((matchData.maxBallCount == matchData.ballCount)) {
            console.log('innings has ended, by ball count.')
            return;
        };

        if ((matchData.battingTeamName.totalBatters < 1) && (Object.keys(matchData.battingTeamName.battingData).length > 0)) {
            console.log('innings is over, by all out.')
            return;
        };

        if (openedInputBox == 'batsmanName' && !(BatterInfoInput == '')) {
            let batsmanTobeAdded = BatterInfoInput;

            if ((matchData.battingTeamName.battingData[batsmanTobeAdded]) || (Object.keys(matchData.battingTeamName.battingData).length == 11)) {
                openedInputBox = false;
                return;
            };

            matchData.battingTeamName.battingData[batsmanTobeAdded] = {
                name: batsmanTobeAdded,
                runs: 0,
                ballsFaced: 0,
                isOut: false,
                foursHit: 0,
                strikeRate: 0,
                sixesHit: 0,
                index: Object.keys(matchData.battingTeamName.battingData).length + 1,
                arrayIndex: matchData.battingTeamName.availableBatters.length + 1
            };
            
            matchData.battingTeamName.availableBatters.push(matchData.battingTeamName.battingData[batsmanTobeAdded].index);
            matchData.battingTeamName.totalBatters += 1


            console.log(matchData.battingTeamName.activeBatsman);
            setTimeout(function () {
                if ((Object.keys(matchData.battingTeamName.battingData).length > 0)) {
                    CreateActivePlayer('batting');
                };
            }, 1000)

            BatterInfoInput = '';
            openedInputBox = false;
            return;
        };


        if (openedInputBox == 'bowlerName' && !(BowlerInfoInput == '')) {
            let bowlerTobeAdded = BowlerInfoInput;

            if ((matchData.bowlingTeamName.bowlingData[bowlerTobeAdded]) || (Object.keys(matchData.bowlingTeamName.bowlingData).length == 11)) {
                openedInputBox = false;
                return;
            };

            matchData.bowlingTeamName.bowlingData[bowlerTobeAdded] = {
                name: bowlerTobeAdded,
                runs: 0,
                oversBowled: 0,
                economy: 0,
                wickets: 0,
                wicketsTaken: [],
                index: Object.keys(matchData.bowlingTeamName.bowlingData).length + 1,
                arrayIndex: matchData.bowlingTeamName.availableBowlers.length + 1
            };

            matchData.bowlingTeamName.availableBowlers.push(matchData.bowlingTeamName.bowlingData[bowlerTobeAdded].index);
            matchData.bowlingTeamName.totalBowlers += 1

            setTimeout(function () {
                if ((Object.keys(matchData.bowlingTeamName.bowlingData).length > 0)) {
                    CreateActivePlayer('bowling');
                };
            }, 1000)

            
            BowlerInfoInput = '';
            openedInputBox = false;
            return;
        };
        openedInputBox = false;
    };

    function ProcessBatsmanStats(p1, p2) {
        let currentActiveBatsman = matchData.battingTeamName.activeBatsman;
        console.log(p1, p2)
        if (p1 == 'add') {
            matchData.battingTeamName.battingData[currentActiveBatsman].runs += p2

            if (p2 == 4) {
                matchData.battingTeamName.battingData[currentActiveBatsman].foursHit += 1
            } else if (p2 == 6) {
                matchData.battingTeamName.battingData[currentActiveBatsman].sixesHit += 1
            };
        }

        if (p1 == 'addBall'){
            matchData.battingTeamName.battingData[currentActiveBatsman].ballsFaced += 1
            matchData.battingTeamName.battingData[currentActiveBatsman].strikeRate = ((matchData.battingTeamName.battingData[currentActiveBatsman].runs / matchData.battingTeamName.battingData[currentActiveBatsman].ballsFaced) * 100).toFixed(1);
        }

        if (p1 == 'wicket') {
            matchData.battingTeamName.battingData[currentActiveBatsman].isOut = true;
            matchData.battingTeamName.battingData[currentActiveBatsman].wicketTakenBy = p2;

            matchData.battingTeamName.availableBatters.splice(matchData.battingTeamName.battingData[currentActiveBatsman].arrayIndex, 1);
            matchData.battingTeamName.totalBatters -= 1

            console.log('remaining batters', matchData.battingTeamName.totalBatters)
            setTimeout(() => {
                SwapBatsmanButton()
            }, 500);
        };
    };

    function ProcessBowlerStats(p1, p2) {
        let currentActiveBowler = matchData.bowlingTeamName.activeBowler;
        console.log(p1, p2)
        if (p1 == 'add') {
            matchData.bowlingTeamName.bowlingData[currentActiveBowler].runs += p2;
        };

        if (p1 == 'over') {
            if ((Object.keys(matchData.battingTeamName.battingData).length > 1)) {
                matchData.bowlingTeamName.needToChangeBowler = true;
            };

            matchData.bowlingTeamName.bowlingData[currentActiveBowler].oversBowled += 1;

            let economy = (matchData.bowlingTeamName.bowlingData[currentActiveBowler].runs / matchData.bowlingTeamName.bowlingData[currentActiveBowler].oversBowled);
            matchData.bowlingTeamName.bowlingData[currentActiveBowler].economy = parseFloat(economy.toFixed(2));
            CreateActivePlayer('bowling');
        };

        if (p1 == 'wicket') {
            matchData.bowlingTeamName.bowlingData[currentActiveBowler].wicketsTaken.push(p2);
            matchData.bowlingTeamName.bowlingData[currentActiveBowler].wickets += 1
        };
    };

    function AddBallConfirmButton(p1, p2) {
        if (matchData.selectedOptionsForBall.length === 0) {
            isBlurred = false;
            openedInputBox = false;
            ballInputContainer.style.display = "none";
            addBallEvent('clear');
            return;
        };

        if (matchData.bowlingTeamName.needToChangeBowler) {
            isBlurred = false;
            openedInputBox = false;
            ballInputContainer.style.display = "none";
            addBallEvent('clear');
            return;
        };

        if ((matchData.bowlingTeamName.bowlingData.length == 0) || (matchData.battingTeamName.battingData.length == 0)) {
            isBlurred = false;
            openedInputBox = false;
            ballInputContainer.style.display = "none";
            addBallEvent('clear');
            return;
        };

        if ((matchData.maxBallCount == matchData.ballCount)) {
            console.log('innings has ended, by ball count.')
            /* FirstInningsEnd(); */
            return;
        };

        let totalRunsThisBall = 0;
        let countBall = true;

        isBlurred = false;
        openedInputBox = false;
        ballInputContainer.style.display = "none";
        
        matchData.selectedOptionsForBall.forEach(function(item, index) {
            console.log('1', item, index)
            console.log((typeof item))
            if (((typeof item) == 'number')) { 
                totalRunsThisBall = (totalRunsThisBall + item);
                ProcessBatsmanStats('add', item);
                ProcessBowlerStats('add', item);
                console.log('2', totalRunsThisBall, index)
            };

            if (item == 'wide') {
                totalRunsThisBall = (totalRunsThisBall + 1);
                ProcessBowlerStats('add', 1);
                countBall = false;
            }

            if (item == 'byes') {
                totalRunsThisBall = (totalRunsThisBall + 1);
                ProcessBatsmanStats('add', 1);
                ProcessBowlerStats('add', 1);
            }

            if (item == 'legbyes') {
                totalRunsThisBall = (totalRunsThisBall + 1);
                ProcessBatsmanStats('add', 1);
                ProcessBowlerStats('add', 1);
            }

            if (item == 'noball') {
                totalRunsThisBall = (totalRunsThisBall + 1);
                ProcessBatsmanStats('add', 1);
                ProcessBowlerStats('add', 1);
                countBall = false;
            };

            if (item == 'wicket') {
                totalRunsThisBall = (totalRunsThisBall + 0);
                ProcessBowlerStats('wicket', matchData.battingTeamName.activeBatsman);
                ProcessBatsmanStats('wicket', matchData.bowlingTeamName.activeBowler);
                matchData.totalWickets += 1;
            };

        });

        if (countBall) {
            matchData.ballCount = (matchData.ballCount + 1);
            matchData.oversCountFlt += 0.1;

            if (matchData.oversCountFlt > 0.5) {
                matchData.oversCountFlt = 0.5;
            };

            ProcessBatsmanStats('addBall');
        };

        matchData.runCount = (matchData.runCount + totalRunsThisBall);
        matchData.currentOver.push(matchData.selectedOptionsForBall);

        if ((matchData.target) && (matchData.runCount >= matchData.target)) {
            matchData.currentOverNumber = (matchData.currentOverNumber + 1);
            matchData.oversData[matchData.currentOverNumber] = matchData.currentOver;
            matchData.currentOver = [];
            console.log('match over, by target reached.')
            SecondInningsEnd();
        };

        if (countBall && (matchData.ballCount % 6) == 0) {
            ProcessBowlerStats('over');
            ProcessBatsmanStats('over');
            matchData.currentOverNumber = (matchData.currentOverNumber + 1);
            matchData.oversData[matchData.currentOverNumber] = matchData.currentOver;
            matchData.currentOver = [];

            matchData.oversCountFlt -= 0.5;

            matchData.oversCount += 1;

            if (matchData.maxBallCount == matchData.ballCount) {
                console.log('innings has ended, by ball count.')
                if (!(firstInningsEnd)) {
                    FirstInningsEnd();
                } else if ((firstInningsEnd)) {
                    SecondInningsEnd();
                };
            };
        };

        matchData.selectedOptionsForBall = [];
        console.log(matchData);
        addBallEvent('clear');
    };

    function addBallEvent(p1, p2) {
        console.log(p1, p2, ZeroRunsButton);

        if (p1 == 'clear') {
            WicketsButton.style.backgroundColor = '#f56464';
            NoBallButton.style.backgroundColor = '#f56464';
            ZeroRunsButton.style.backgroundColor = '';
            OneRunsButton.style.backgroundColor = '';
            TwoRunsButton.style.backgroundColor = '';
            ThreeRunsButton.style.backgroundColor = '';
            FourRunsButton.style.backgroundColor = '';
            FiveRunsButton.style.backgroundColor = '';
            SixRunsButton.style.backgroundColor = '';
            ByeRunsButton.style.backgroundColor = '';
            LegByeRunsButton.style.backgroundColor = '';
            WideRunsButton.style.backgroundColor = '';
            matchData.selectedExclusiveOptions = false;
            return;
        };

        if ((matchData.battingTeamName.totalBatters < 1) && (Object.keys(matchData.battingTeamName.battingData).length > 0)) {
            console.log('innings is over, by all out.')
            return;
        };

        if (matchData.maxBallCount == matchData.ballCount) {
            console.log('innings has ended, by ball count.')
            return;
        };

        if ((matchData.battingTeamName.activeBatsman == null) || (matchData.bowlingTeamName.activeBowler == null)) {
            return;
        }

        if (p1 == 'wicket') {
            let button = WicketsButton;
            let index = matchData.selectedOptionsForBall.indexOf('wicket')

            if (!(index == -1)) {
                matchData.selectedOptionsForBall.splice(index, 1);
                button.style.backgroundColor = '#f56464';
                matchData.selectedExclusiveOptions = false;
                return;
            }

            button.style.backgroundColor = '#c9ffd4';
            matchData.selectedOptionsForBall.push('wicket');
            matchData.selectedExclusiveOptions = true;
            return;
        };

        if (p1 == 'runs') {
            if (p2 == 0) {
                let button = ZeroRunsButton;
                let index = matchData.selectedOptionsForBall.indexOf('0')

                if (!(index == -1)) {
                    matchData.selectedOptionsForBall.splice(index, 1);
                    button.style.backgroundColor = '';
                    matchData.selectedExclusiveOptions = false;
                    return;
                }

                button.style.backgroundColor = '#c9ffd4';
                matchData.selectedOptionsForBall.push('0');
                matchData.selectedExclusiveOptions = true;
                return;
            };

            if (p2 == 1 && !(matchData.selectedExclusiveOptions)) {
                let button = OneRunsButton;
                let index = matchData.selectedOptionsForBall.indexOf(1)

                if (!(index == -1)) {
                    matchData.selectedOptionsForBall.splice(index, 1);
                    button.style.backgroundColor = '';
                    return;
                }

                button.style.backgroundColor = '#c9ffd4';
                matchData.selectedOptionsForBall.push(1);
                matchData.selectedExclusiveOptions = false;
                return;
            };

            if (p2 == 2 && !(matchData.selectedExclusiveOptions)) {
                let button = TwoRunsButton;
                let index = matchData.selectedOptionsForBall.indexOf(2)

                if (!(index == -1)) {
                    matchData.selectedOptionsForBall.splice(index, 1);
                    button.style.backgroundColor = '';
                    return;
                }

                button.style.backgroundColor = '#c9ffd4';
                matchData.selectedOptionsForBall.push(2);
                matchData.selectedExclusiveOptions = false;
                return;
            };

            if (p2 == 3 && !(matchData.selectedExclusiveOptions)) {
                let button = ThreeRunsButton;
                let index = matchData.selectedOptionsForBall.indexOf(3)

                if (!(index == -1)) {
                    matchData.selectedOptionsForBall.splice(index, 1);
                    button.style.backgroundColor = '';
                    return;
                }

                button.style.backgroundColor = '#c9ffd4';
                matchData.selectedOptionsForBall.push(3);
                matchData.selectedExclusiveOptions = false;
                return;
            };

            if (p2 == 4 && !(matchData.selectedExclusiveOptions)) {
                let button = FourRunsButton;
                let index = matchData.selectedOptionsForBall.indexOf(4)

                if (!(index == -1)) {
                    matchData.selectedOptionsForBall.splice(index, 1);
                    button.style.backgroundColor = '';
                    return;
                }

                button.style.backgroundColor = '#c9ffd4';
                matchData.selectedOptionsForBall.push(4);
                matchData.selectedExclusiveOptions = false;
                return;
            };

            if (p2 == 5 && !(matchData.selectedExclusiveOptions)) {
                let button = FiveRunsButton;
                let index = matchData.selectedOptionsForBall.indexOf(5)

                if (!(index == -1)) {
                    matchData.selectedOptionsForBall.splice(index, 1);
                    button.style.backgroundColor = '';
                    return;
                }

                button.style.backgroundColor = '#c9ffd4';
                matchData.selectedOptionsForBall.push(5);
                matchData.selectedExclusiveOptions = false;
                return;
            };

            if (p2 == 6 && !(matchData.selectedExclusiveOptions)) {
                let button = SixRunsButton;
                let index = matchData.selectedOptionsForBall.indexOf(6)

                if (!(index == -1)) {
                    matchData.selectedOptionsForBall.splice(index, 1);
                    button.style.backgroundColor = '';
                    return;
                }

                button.style.backgroundColor = '#c9ffd4';
                matchData.selectedOptionsForBall.push(6);
                matchData.selectedExclusiveOptions = false;
                return;
            };

            if (p2 == 'wide' && !(matchData.selectedExclusiveOptions)) {
                let button = WideRunsButton;
                let index = matchData.selectedOptionsForBall.indexOf('wide')

                if (!(index == -1)) {
                    matchData.selectedOptionsForBall.splice(index, 1);
                    button.style.backgroundColor = '';
                    return;
                }

                button.style.backgroundColor = '#c9ffd4';
                matchData.selectedOptionsForBall.push('wide');
                matchData.selectedExclusiveOptions = false;
                return;
            };


            if (p2 == 'byes' && !(matchData.selectedExclusiveOptions)) {
                let button = ByeRunsButton;
                let index = matchData.selectedOptionsForBall.indexOf('byes')

                if (!(index == -1)) {
                    matchData.selectedOptionsForBall.splice(index, 1);
                    button.style.backgroundColor = '';
                    return;
                }

                button.style.backgroundColor = '#c9ffd4';
                matchData.selectedOptionsForBall.push('byes');
                matchData.selectedExclusiveOptions = false;
                return;
            };

            if (p2 == 'legbyes' && !(matchData.selectedExclusiveOptions)) {
                let button = LegByeRunsButton;
                let index = matchData.selectedOptionsForBall.indexOf('legbyes')

                if (!(index == -1)) {
                    matchData.selectedOptionsForBall.splice(index, 1);
                    button.style.backgroundColor = '';
                    return;
                }

                button.style.backgroundColor = '#c9ffd4';
                matchData.selectedOptionsForBall.push('legbyes');
                matchData.selectedExclusiveOptions = false;
                return;
            };

            if (p2 == 'noball' && !(matchData.selectedExclusiveOptions)) {
                let button = NoBallButton;
                let index = matchData.selectedOptionsForBall.indexOf('noball')

                if (!(index == -1)) {
                    matchData.selectedOptionsForBall.splice(index, 1);
                    button.style.backgroundColor = '#f56464';
                    return;
                }

                button.style.backgroundColor = '#c9ffd4';
                matchData.selectedOptionsForBall.push('noball');
                matchData.selectedExclusiveOptions = false;
                return;
            };
        }
    };

    function SwapBatsmanButton(p1, p2) {
        if ((matchData.battingTeamName.totalBatters < 1) && !(matchData.battingTeamName.activeBatsman == null) && ((Object.keys(matchData.battingTeamName.battingData).length > 0))) {
            console.log('innings is over, by all out.')
            matchData.battingTeamName.battingData[matchData.battingTeamName.activeBatsman].name = matchData.battingTeamName.battingData[matchData.battingTeamName.activeBatsman].name.slice(0, -1);
            matchData.battingTeamName.activeBatsman = null;
            if (!(firstInningsEnd)) {
                FirstInningsEnd();
            } else if ((firstInningsEnd)) {
                SecondInningsEnd();
            };
            return;
        };

        if ((Object.keys(matchData.battingTeamName.battingData).length == 1)) {
            return;
        };

        if ((matchData.maxBallCount == matchData.ballCount)) {
            console.log('innings has ended, by ball count.')
            return;
        };

        if ((matchData.battingTeamName.totalBatters < 1) && (Object.keys(matchData.battingTeamName.battingData).length > 0)) {
            console.log('innings is over, by all out.')
            return;
        };

        if (!(matchData.battingTeamName.activeBatsmanIndex == null)) {
            let newIndex = matchData.battingTeamName.activeBatsmanIndex + 1

            if (p2) {
                newIndex = p2;
            };
            
            if ((newIndex > Object.keys(matchData.battingTeamName.battingData).length)) {
                newIndex = 1;
            };

            let playerAtNewIndex = PlayerAtIndex('batting', newIndex);

            if (matchData.battingTeamName.battingData[playerAtNewIndex].isOut == true) {
                console.log('this player is out');
                SwapBatsmanButton(false, (newIndex+1))
                return;
            };

            matchData.battingTeamName.battingData[matchData.battingTeamName.activeBatsman].name = matchData.battingTeamName.battingData[matchData.battingTeamName.activeBatsman].name.slice(0, -1);

            let newName = matchData.battingTeamName.battingData[playerAtNewIndex].name + '*'

            matchData.battingTeamName.activeBatsman = playerAtNewIndex;
            matchData.battingTeamName.battingData[playerAtNewIndex].name = newName
            matchData.battingTeamName.activeBatsmanIndex = matchData.battingTeamName.battingData[playerAtNewIndex].index
        }
    };

    function PlayerAtIndex(p1, p2) {
        if (p1 == 'batting') {
            for (let name in matchData.battingTeamName.battingData) {
                console.log(name, p2)
                if ((matchData.battingTeamName.battingData[name].index == p2)) {
                    return name;
                };
            };
        };

        if (p1 == 'bowling') {
            for (let name in matchData.bowlingTeamName.bowlingData) {
                if ((matchData.bowlingTeamName.bowlingData[name].index == p2)) {
                    return name;
                };
            };
        };
    };

    function SwapBowlerButton() {
        if ((Object.keys(matchData.bowlingTeamName.bowlingData).length == 1)) {
            return;
        };

        if (matchData.maxBallCount == matchData.ballCount) {
            console.log('innings has ended, by ball count.')
            return;
        };

        if ((matchData.battingTeamName.totalBatters < 1) && (Object.keys(matchData.battingTeamName.battingData).length > 0)) {
            console.log('innings is over, by all out.')
            return;
        };

        if (!(matchData.bowlingTeamName.activeBowlerIndex == null)) {
            let newIndex = (matchData.bowlingTeamName.activeBowlerIndex + 1)

            matchData.bowlingTeamName.bowlingData[matchData.bowlingTeamName.activeBowler].name = matchData.bowlingTeamName.bowlingData[matchData.bowlingTeamName.activeBowler].name.slice(0, -1);
            matchData.bowlingTeamName.previousBowler = matchData.bowlingTeamName.activeBowler;

            if ((newIndex > Object.keys(matchData.bowlingTeamName.bowlingData).length)) {
                newIndex = 1;
            };

            let playerAtNewIndex = PlayerAtIndex('bowling', newIndex);
            let newName = matchData.bowlingTeamName.bowlingData[playerAtNewIndex].name + '*'

            matchData.bowlingTeamName.activeBowler = playerAtNewIndex;
            matchData.bowlingTeamName.needToChangeBowler = false;
            matchData.bowlingTeamName.bowlingData[playerAtNewIndex].name = newName
            matchData.bowlingTeamName.activeBowlerIndex = matchData.bowlingTeamName.bowlingData[playerAtNewIndex].index
        }
    };

    function makeBowlersBatters() {
        for (let name in matchData.battingTeamName.bowlingData) {
            matchData.battingTeamName.battingData[name] = {
                name: name,
                runs: 0,
                ballsFaced: 0,
                isOut: false,
                foursHit: 0,
                strikeRate: 0,
                sixesHit: 0,
                index: Object.keys(matchData.battingTeamName.battingData).length + 1,
                arrayIndex: matchData.battingTeamName.availableBatters.length + 1
            };

            matchData.battingTeamName.availableBatters.push(matchData.battingTeamName.battingData[name].index);
            matchData.battingTeamName.totalBatters += 1
        };

        CreateActivePlayer('batting');
    };

    function makeBattersBowlers(p1) {
        for (let name in matchData.bowlingTeamName.battingData) {
            matchData.bowlingTeamName.bowlingData[name] = {
                name: name,
                runs: 0,
                oversBowled: 0,
                economy: 0,
                wickets: 0,
                wicketsTaken: [],
                index: Object.keys(matchData.bowlingTeamName.bowlingData).length + 1,
                arrayIndex: matchData.bowlingTeamName.availableBowlers.length + 1
            }; 


            matchData.bowlingTeamName.availableBowlers.push(matchData.bowlingTeamName.bowlingData[name].index);
            matchData.bowlingTeamName.totalBowlers += 1
        };

        CreateActivePlayer('bowling');
    };

    function FirstInningsEnd() {
        let bowlingDataCopy = matchData.bowlingTeamName;

        matchData.battingTeamName.runsScored = matchData.runCount;
        matchData.battingTeamName.ballsFaced = matchData.ballCount;
        matchData.battingTeamName.wicketsFallen = matchData.totalWickets;
        matchData.battingTeamName.oversCountFlt = matchData.oversCountFlt;
        matchData.battingTeamName.oversCount = matchData.oversCount; 

        setTimeout(() => {
            matchData.bowlingTeamName = matchData.battingTeamName;
            matchData.battingTeamName = bowlingDataCopy;

            bowlingTeamName = battingTeamName;
            battingTeamName = (bowlingTeamName == teamOneName) ? teamTwoName : teamOneName;

            matchData.target = (matchData.runCount + 1);

            matchData.ballCount = 0;
            matchData.runCount = 0;
            matchData.totalWickets = 0;
            matchData.oversCountFlt = 0;
            matchData.oversCount = 0;

            firstInningsEnd = true;

            matchData.battingTeamName.activeBatsman = null;
            matchData.bowlingTeamName.activeBowler = null;

            makeBowlersBatters();
            makeBattersBowlers();

            console.log('first innings end', matchData);
        }, 1000)
    };

    function SecondInningsEnd() {
        setTimeout(() => {
            matchData.battingTeamName.runsScored = matchData.runCount;
            matchData.battingTeamName.ballsFaced = matchData.ballCount;
            matchData.battingTeamName.wicketsFallen = matchData.totalWickets;
            matchData.battingTeamName.oversCountFlt = matchData.oversCountFlt;
            matchData.battingTeamName.oversCount = matchData.oversCount; 

            let winningTeam;
            let winningCause;

            if ((matchData.battingTeamName.runsScored > matchData.bowlingTeamName.runsScored)) {
                winningTeam = battingTeamName;

                winningCause = battingTeamName + ' won by ' + (((Object.keys(matchData.bowlingTeamName.bowlingData).length) == matchData.battingTeamName.totalBatters) ? matchData.battingTeamName.totalBatters : ((Object.keys(matchData.bowlingTeamName.bowlingData).length) - matchData.battingTeamName.totalBatters)) + ' wickets.'
            } else if ((matchData.battingTeamName.runsScored < matchData.bowlingTeamName.runsScored)) {
                winningTeam = bowlingTeamName;

                let oversRemain = (oversPerInnings - matchData.battingTeamName.oversCount);
                let oversRemainFlt = (0.6 - matchData.battingTeamName.oversCountFlt);
                let oversRemainTotal = (oversRemain + oversRemainFlt).toFixed(1);

                let final = (oversRemainTotal == 0.6) ? 'no' : oversRemainTotal

                winningCause = bowlingTeamName + ' won by ' + (matchData.target - matchData.battingTeamName.runsScored) + ' runs.';
            };


            if ((matchData.battingTeamName.runsScored == matchData.bowlingTeamName.runsScored)) {
                winningTeam = 'Match Draw';
                winningCause = 'Match Draw';
            };



            overallScoreCard.innerHTML = '<div style="font-family: Outfit; font-size: 1.3rem; margin-left: 10px;">' + winningCause + '</div>';
            MatchContainer.style.display = 'none'
            console.log('second innings end', matchData, winningCause);
        }, 500)
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

    <div class="overall-scorecard" bind:this={overallScoreCard} style="display:none">
        <div class="batting-team-name-overall-score" style="font-family: Outfit; text-overflow: ellipsis; white-space: nowrap; overflow: hidden; margin-left: 15px; min-width: 100px; max-width: 100px; text-align: center; font-size: 1.4rem; flex-grow: 0; flex-shrink: 0; font-weight: 600;">{battingTeamName}</div>
        <div class="target-set" style="font-family: Outfit; flex-grow: 0; position: absolute; flex-shrink: 0; left: 120px; width: 100px; text-align: center; font-size: 1.2rem; font-weight: 600;">{(matchData.target ? 'To Win ' + matchData.target : '')}</div>
        <div class="batting-team-overall-score" style="font-family: Outfit; left: 230px; position: absolute; width: 50px; text-align: center; font-size: 1.2rem; flex-grow: 0; flex-shrink: 0; font-weight: 600;">{matchData.runCount}/{matchData.totalWickets} </div>
        <div class="overs-count-overall" style="font-family: Outfit; flex-grow: 0; left: 290px; position: absolute; flex-shrink: 0; width: 50px; text-align: center; font-size: 1.2rem; font-weight: 600;">{((matchData.oversCount + matchData.oversCountFlt).toFixed(1))}</div>
    </div>

    <div class="match-container" bind:this={MatchContainer} style="display:none">
        <div class="match-title" bind:this={matchTitle} style="font-family: Outfit; font-size: 2rem; padding-top: 10px; font-weight: 500; user-select:none; text-align:center; color:rgba(255, 255, 255, 0.8);">Match</div>
        <div class="batting-scorecard" class:blurred={isBlurred}>
            <div class="batting-team-name">
                Now Batting {battingTeamName}
            </div>

            <div class="stats-describer" style="display: flex; flex-direction: row; flex-wrap: nowrap; margin-top: -5px; align-content: center; justify-content: center; align-items: center; position: absolute; font-family: Outfit; font-size: 0.8rem; /* z-index: 2; */ margin-left: 140px; word-spacing: 12px;">Runs  Balls  Sixes  S/R</div>

            <div class="batting-team-scorebox" bind:this={BattingTeamScorebox}>
                {#if matchData && matchData.battingTeamName && matchData.battingTeamName.battingData}
                    {#each Object.entries(matchData.battingTeamName.battingData) as [name, stats]}
                        {#if name && stats}
                            <div class="batsman-info" style="width: 300px; margin-top: 10px; height: 25px; min-height: 25px; display: flex; background-color: #ffffff75; border-radius: 3px; flex-wrap: nowrap; flex-direction: row; gap: 40px; justify-content: center; /* align-content: center; */ align-items: center;}">
                                <div class="batsman-name" style="color: rgba(0, 0, 0, 0.8); width: 70px; margin-left: 5px; font-family: Outfit; font-size: 1rem; font-weight: 600;">{stats.name}</div>
                                <div class="batsman-stats" style="display: flex; justify-content: center; align-content: center; flex-wrap: nowrap; flex-direction: row; width: 170px; align-items: center;">
                                    <div class="runs-scored" style=" left: 100px; color: rgba(0, 0, 0, 0.8); font-family: Outfit; font-size: 1rem; text-align: center; min-width: 40px; max-width: 40px; flex-shrink: 0; flex-grow: 0; font-weight: 600;">{stats.runs}</div>
                                    <div class="balls-faced" style=" left: 140px; color: rgba(0, 0, 0, 0.8); font-family: Outfit; font-size: 1rem; text-align: center; min-width: 40px; max-width: 40px; flex-shrink: 0; flex-grow: 0; font-weight: 600;">{stats.ballsFaced}</div>
                                    <div class="fours-hit" style=" left: 160px; color: rgba(0, 0, 0, 0.8); font-family: Outfit; font-size: 1rem; text-align: center; min-width: 40px; max-width: 40px; flex-shrink: 0; flex-grow: 0; font-weight: 600;">{stats.sixesHit}</div>
                                    <div class="sixes-hit" style=" left: 200px; color: rgba(0, 0, 0, 0.8); font-family: Outfit; font-size: 1rem; text-align: center; min-width: 40px; max-width: 40px; flex-shrink: 0; flex-grow: 0; font-weight: 600;">{stats.strikeRate}</div>
                                </div>

                            </div>
                        {/if}
                    {/each}
                {/if}
            </div>
        </div>
        
        <div class="bowling-scorecard" class:blurred={isBlurred}>
            <div class="bowling-team-name">
                Now Bowling {bowlingTeamName}
            </div>
            
            <div class="stats-describer-bowling" style="display: flex; flex-direction: row; margin-top: -5px; flex-wrap: nowrap; align-content: center; justify-content: center; align-items: center; position: absolute; font-size: 0.8rem; font-family: Outfit; /* z-index: 2; */ margin-left: 125px; word-spacing: 5px;">Overs  Runs  Economy  Wickets</div>

            <div class="bowling-team-scorebox">
                {#if matchData && matchData.bowlingTeamName && matchData.bowlingTeamName.bowlingData}
                    {#each Object.entries(matchData.bowlingTeamName.bowlingData) as [name, stats]}
                        {#if !(name == '') && stats}
                            <div class="bowler-info" style="width: 300px; margin-top: 10px; height: 25px; min-height: 25px; display: flex; background-color: #ffffff75; border-radius: 3px; flex-wrap: nowrap; flex-direction: row; gap: 40px; justify-content: center; /* align-content: center; */ align-items: center;}">
                                <div class="bowler-name" style="color: rgba(0, 0, 0, 0.8); width: 70px; margin-left: 5px; font-family: Outfit; font-size: 1rem; font-weight: 600;">{stats.name}</div>
                                <div class="bowler-stats" style="display: flex; justify-content: center; align-content: center; flex-wrap: nowrap; gap: 10px; width: 170px; flex-direction: row; align-items: center;">
                                    <div class="runs-scored" style=" left: 100px; color: rgba(0, 0, 0, 0.8); font-family: Outfit; font-size: 1rem; text-align: center; min-width: 40px; max-width: 40px; flex-shrink: 0; flex-grow: 0; font-weight: 600;">{stats.oversBowled}</div>
                                    <div class="balls-faced" style=" left: 140px; color: rgba(0, 0, 0, 0.8); font-family: Outfit; font-size: 1rem; text-align: center; min-width: 40px; max-width: 40px; flex-shrink: 0; flex-grow: 0; font-weight: 600;">{stats.runs}</div>
                                    <div class="fours-hit" style=" left: 160px; color: rgba(0, 0, 0, 0.8); font-family: Outfit; font-size: 1rem; text-align: center; min-width: 40px; max-width: 40px; flex-shrink: 0; flex-grow: 0; font-weight: 600;">{stats.economy}</div>
                                    <div class="sixes-hit" style=" left: 200px; color: rgba(0, 0, 0, 0.8); font-family: Outfit; font-size: 1rem; text-align: center; min-width: 40px; max-width: 40px; flex-shrink: 0; flex-grow: 0; font-weight: 600;">{stats.wickets}</div>
                                </div>

                            </div>
                        {/if}
                    {/each}
                {/if}
            </div>
        </div>


        <div class="info-input-container" style="display:none" bind:this={infoInputContainer}>
            <div class="batter-info-box" style="display:none" bind:this={batterInfoBox}>
                <input class="batter-info-input" placeholder="Ke Namtese?" type="text" minlength="4" maxlength="8" bind:value={BatterInfoInput}> 
            </div>

            <div class="bowler-info-box" style="display:none" bind:this={bowlerInfoBox}>
                <input class="bowler-info-input" placeholder="Ke Dheelabe?" type="text" minlength="4" maxlength="8" bind:value={BowlerInfoInput}> 
            </div>

            <input class="button-submit-box" type="button" value="Thik Aseh!" bind:this={buttonSubmit} on:click={SubmitButton}>
        </div>

        <div class="ball-input-container" style="display:none" bind:this={ballInputContainer}>
            <div class="ball-input-button-container" style="display: flex; height: 140px; width: 100%; flex-direction: row; flex-wrap: wrap; align-content: center; justify-content: center; gap: 5px; align-items: center;">
                <input class="button-add-ball" type="button" style="height: 30px; width: 80px; color: rgba(0, 0, 0, 0.8); font-family: Outfit; font-size: 1rem; text-align: center; font-weight: 600; border-radius: 5px; border: 0; outline: 0;" bind:this={ZeroRunsButton} value="0" on:click={a => addBallEvent("runs", 0)}>
                <input class="button-add-ball" type="button" style="height: 30px; width: 80px; color: rgba(0, 0, 0, 0.8); font-family: Outfit; font-size: 1rem; text-align: center; font-weight: 600; border-radius: 5px; border: 0; outline: 0;" bind:this={OneRunsButton} value="1" on:click={a => addBallEvent("runs", 1)}>
                <input class="button-add-ball" type="button" style="height: 30px; width: 80px; color: rgba(0, 0, 0, 0.8); font-family: Outfit; font-size: 1rem; text-align: center; font-weight: 600; border-radius: 5px; border: 0; outline: 0;" bind:this={TwoRunsButton} value="2" on:click={a => addBallEvent("runs", 2)}>
                <input class="button-add-ball" type="button" style="height: 30px; width: 80px; color: rgba(0, 0, 0, 0.8); font-family: Outfit; font-size: 1rem; text-align: center; font-weight: 600; border-radius: 5px; border: 0; outline: 0;" bind:this={ThreeRunsButton} value="3" on:click={a => addBallEvent("runs", 3)}>
                <input class="button-add-ball" type="button" style="height: 30px; width: 80px; color: rgba(0, 0, 0, 0.8); font-family: Outfit; font-size: 1rem; text-align: center; font-weight: 600; border-radius: 5px; border: 0; outline: 0;" bind:this={FourRunsButton}  value="4" on:click={a => addBallEvent("runs", 4)}>
                <input class="button-add-ball" type="button" style="height: 30px; width: 80px; color: rgba(0, 0, 0, 0.8); font-family: Outfit; font-size: 1rem; text-align: center; font-weight: 600; border-radius: 5px; border: 0; outline: 0;" bind:this={FiveRunsButton} value="5" on:click={a => addBallEvent("runs", 5)}>
                <input class="button-add-ball" type="button" style="height: 30px; width: 80px; color: rgba(0, 0, 0, 0.8); font-family: Outfit; font-size: 1rem; text-align: center; font-weight: 600; border-radius: 5px; border: 0; outline: 0;" bind:this={SixRunsButton} value="6" on:click={a => addBallEvent("runs", 6)}>
                <input class="button-add-ball" type="button" style="height: 30px; width: 80px; color: rgba(0, 0, 0, 0.8); font-family: Outfit; font-size: 1rem; text-align: center; font-weight: 600; border-radius: 5px; border: 0; outline: 0;" bind:this={WideRunsButton} value="Wide" on:click={a => addBallEvent("runs", "wide")}>
                <input class="button-add-ball" type="button" style="height: 30px; width: 80px; color: rgba(0, 0, 0, 0.8); font-family: Outfit; font-size: 1rem; text-align: center; font-weight: 600; border-radius: 5px; border: 0; outline: 0; background-color:#f56464;" bind:this={NoBallButton} value="No Ball" on:click={a => addBallEvent("runs", 'noball')}>
                <input class="button-add-ball" type="button" style="height: 30px; width: 80px; color: rgba(0, 0, 0, 0.8); font-family: Outfit; font-size: 1rem; text-align: center; font-weight: 600; border-radius: 5px; border: 0; outline: 0;" bind:this={ByeRunsButton} value="Byes" on:click={a => addBallEvent("runs", 'byes')}>
                <input class="button-add-ball" type="button" style="height: 30px; width: 80px; color: rgba(0, 0, 0, 0.8); font-family: Outfit; font-size: 1rem; text-align: center; font-weight: 600; border-radius: 5px; border: 0; outline: 0;" bind:this={LegByeRunsButton} value="Leg Byes" on:click={a => addBallEvent("runs", 'legbyes')}>
                <input class="button-add-ball" type="button" style="height: 30px; width: 80px; color: rgba(0, 0, 0, 0.8); font-family: Outfit; font-size: 1rem; text-align: center; font-weight: 600; border-radius: 5px; border: 0; outline: 0; background-color:#f56464;" bind:this={WicketsButton} value="Wicket" on:click={a => addBallEvent("wicket")}>    
            </div>
       
            <input class="button-add-ball" type="button" value="Proceed!" on:click={AddBallConfirmButton}>
        </div>

        <input class="button-batsman-modifier" class:blurred={isBlurred} type="button" value="Add Batsman" bind:this={buttonBatsmanModifier} on:click={BatsmanButton}>
        <input class="button-bowler-modifier"class:blurred={isBlurred}  type="button" value="Add Bowler" bind:this={buttonBowlerModifier} on:click={BowlerButton}>
        <input class="button-ball-modifier" class:blurred={isBlurred} type="button" value="Add Ball" bind:this={buttonBallsModifier} on:click={AddBallButton}>
        <input class="button-swap-batter-modifier" class:blurred={isBlurred} type="button" value="Swap Batsman" on:click={SwapBatsmanButton}>
        <input class="button-swap-bowler-modifier" class:blurred={isBlurred} type="button" value="Swap Bowler" on:click={SwapBowlerButton}>
    </div>
</div>


<style>

    ::-webkit-scrollbar {
        width: 4px;
    }

    /* Track */
    ::-webkit-scrollbar-track {
        background: #ffffff00;
        border-radius: 1px;
    }

    /* Handle */
    ::-webkit-scrollbar-thumb {
        background: #88888800;
    }

    /* Handle on hover */
    ::-webkit-scrollbar-thumb:hover {
        background: #ffffff00;
    }

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

    .info-input-container{
        height: 100px;
        width: 200px;
        background-color: #ffffff75;
        display: flex;
        flex-direction: column;
        flex-wrap: nowrap;
        align-content: center;
        justify-content: center;
        border-radius: 5px;
        align-items: center;
        z-index: 500;
        top: 40%;
        gap: 20px;
        position: absolute;
    }

    .ball-input-container{
        height: 200px;
        width: 280px;
        background-color: #ffffff75;
        display: flex;
        flex-direction: column;
        flex-wrap: nowrap;
        align-content: center;
        justify-content: center;
        border-radius: 5px;
        align-items: center;
        z-index: 500;
        top: 40%;
        gap: 20px;
        position: absolute;
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
        width: 120px;
        color: rgba(0, 0, 0, 0.8);
        font-family: Outfit;
        font-size: 1rem;
        text-align: center;
        font-weight: 600;
        border-radius: 5px;
        border: 0;
        /* padding-bottom: 10px; */
        outline: 0;
        top: 500px;
        right: 215px;
        position: absolute;
    }

    .button-batsman-modifier:active{
        scale: 0.98;
    }

    .button-bowler-modifier{
        height: 30px;
        width: 120px;
        color: rgba(0, 0, 0, 0.8);
        font-family: Outfit;
        font-size: 1rem;
        text-align: center;
        font-weight: 600;
        border-radius: 5px;
        border: 0;
        /* padding-bottom: 10px; */
        outline: 0;
        top: 500px;
        left: 215px;
        position: absolute;
    }

    .button-bowler-modifier:active{
        scale: 0.98;
    }

    .button-submit-box:active{
        scale: 0.98;
    }

    .button-add-ball:active{
        scale: 0.98;
    }

    .button-ball-modifier{
        height: 40px;
        width: 320px;
        color: rgba(0, 0, 0, 0.8);
        font-family: Outfit;
        font-size: 1rem;
        text-align: center;
        font-weight: 600;
        border-radius: 5px;
        border: 0;
        /* padding-bottom: 10px; */
        outline: 0;
        top: 590px;
        /* right: 220px; */
        position: absolute;
    }

    .button-ball-modifier:active{
        scale: 0.98;
    }

    .button-swap-batter-modifier{
        height: 30px;
        width: 120px;
        color: rgba(0, 0, 0, 0.8);
        font-family: Outfit;
        font-size: 1rem;
        text-align: center;
        font-weight: 600;
        border-radius: 5px;
        border: 0;
        /* padding-bottom: 10px; */
        outline: 0;
        top: 545px;
        right: 215px;
        position: absolute;
    }

    .button-swap-batter-modifier:active{
        scale: 0.98;
    }

    .button-swap-bowler-modifier{
        height: 30px;
        width: 120px;
        color: rgba(0, 0, 0, 0.8);
        font-family: Outfit;
        font-size: 1rem;
        text-align: center;
        font-weight: 600;
        border-radius: 5px;
        border: 0;
        /* padding-bottom: 10px; */
        outline: 0;
        left: 215px;
        top: 545px;
        position: absolute;
    }

    .button-swap-bowler-modifier:active{
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
        padding-bottom: 5px;
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
        justify-content: flex-start;
        align-items: center;
        height: 150px;
        max-height: 150px;
        width: 320px;
        border-radius: 5px;
        background-color: #ffffff4d;
        padding-bottom: 10px;
        overflow-y: scroll;
        margin-top: 20px;
    }

    .bowling-team-name{
        font-family: Outfit;
        font-size: 1.2rem;
        padding-top: 10px;
        font-weight: 500;
        user-select: none;
        text-align: center;
        padding-bottom: 5px;
        color: rgb(255 255 255);
        margin-bottom: 5px;
    }

    .bowling-team-scorebox{
        display: flex;
        flex-direction: column;
        flex-wrap: nowrap;
        justify-content: flex-start;
        align-items: center;
        height: 150px;
        max-height: 150px;
        width: 320px;
        border-radius: 5px;
        background-color: #ffffff4d;
        padding-bottom: 10px;
        overflow-y: scroll;
        margin-top: 20px;
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