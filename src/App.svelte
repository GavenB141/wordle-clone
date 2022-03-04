<script>
	import Key from './Key.svelte';
	import Letter from './Letter.svelte';
	import Alert from './Alert.svelte';
	import Keydown from "svelte-keydown";

	let dictionary = ["LASER", "DROID", "ROBOT", "FIRST", "SCREW", "MOTOR", "WHEEL", "CUBEY", "BUILD", "CLIMB", "SHOOT", "DRIVE", "SCORE", "POWER", "POINT", "CARGO", "INDEX", "WIRES", "BOLTS", "GEARS", "FRAME", "METAL", "SCOUT", "TEAMS", "MEDIA", "RAVEN", "LIGHT", "VIDEO", "RULES", "TECHS", "CODED", "TOOLS", "OILED", "HOODS", "DRILL", "CHEER", "COACH", "FINAL", "CHAMP", "AXLES", "BEAMS", "STATS", "FOULS", "BLADE", "RUNGS", "ROTOR", "SHEET", "BLOCK", "REACT", "SERVO", "DEBUG", "ERROR", "VALVE", "BALLS", "SHAFT", "TESTS", "WIRED", "TUBES", "PILOT", "ROVER", "DRONE", "GIZMO", "PROBE", "CYBER", "MICRO", "JOINT", "CHAIN", "LINKS", "GOALS", "SHIFT", "PIVOT", "SWING"];

	//let word = "robot".toUpperCase();
	let word = dictionary[Math.floor(Math.random() * dictionary.length)];
	let len = word.length;
	let wordArray = word.split("");
	let guesses = 6;
	let done = false;

	//colors
	let dk= "gray-300";  //default keys
	let d = "gray-500 "; //default board
	let i = "gray-500";  //incorrect
	let c = "green-500"; //correct
	let p = "amber-400"; //partial

	let opacity = "100";

	let keyboard = [
		["Q","W","E","R","T","Y","U","I","O","P"],
		["A","S","D","F","G","H","J","K","L"],
		["ENTER","Z","X","C","V","B","N","M","BACK"]
	];

	let keyColors = {
		"Q":dk,"W":dk,"E":dk,"R":dk,"T":dk,"Y":dk,"U":dk,"I":dk,"O":dk,"P":dk,
		"A":dk,"S":dk,"D":dk,"F":dk,"G":dk,"H":dk,"J":dk,"K":dk,"L":dk,
		"ENTER":dk,"Z":dk,"X":dk,"C":dk,"V":dk,"B":dk,"N":dk,"M":dk,"BACK":dk
	};

	let board = [];

	for(let j = 0; j < guesses; j++){
		board.push([]);
	}

	for(let a = 0; a < board.length; a++){
		for(let b = 0; b < len; b++){
			board[a].push({value:"",color:d});
		}
	}

	let cursor = {x:0,y:0};

	function handleInput(event){
		processInput(event.detail.input);
	}

	function processInput(input){
		if(cursor.y < guesses && !done){
			input = input.toUpperCase();
			
			if(input=="ENTER"){
				enter();
			}else if(input=="BACK" || input=="BACKSPACE"){
				backspace();
			}else if(input.length == 1 && (/[A-Z]/).test(input)){
				sendLetter(input);
			}
		}
	}

	function sendLetter(input){
		if(cursor.x < len){
			board[cursor.y][cursor.x].value = input;
			cursor.x += 1;
		}
	}

	function backspace(){
		if(cursor.x > 0){
			cursor.x -= 1;
			board[cursor.y][cursor.x].value = "";
		}
	}

	/**
	 * Process full word input
	 */
	function enter(){
		if(cursor.x == len){
			let numLetters = {
				"Q":{word:0,answer:0},"W":{word:0,answer:0},"E":{word:0,answer:0},"R":{word:0,answer:0},"T":{word:0,answer:0},"Y":{word:0,answer:0},"U":{word:0,answer:0},"I":{word:0,answer:0},"O":{word:0,answer:0},"P":{word:0,answer:0},
				"A":{word:0,answer:0},"S":{word:0,answer:0},"D":{word:0,answer:0},"F":{word:0,answer:0},"G":{word:0,answer:0},"H":{word:0,answer:0},"J":{word:0,answer:0},"K":{word:0,answer:0},"L":{word:0,answer:0},
				"Z":{word:0,answer:0},"X":{word:0,answer:0},"C":{word:0,answer:0},"V":{word:0,answer:0},"B":{word:0,answer:0},"N":{word:0,answer:0},"M":{word:0,answer:0}
			};

			//Get the full answer and record the number of each letter for proper coloring, and check for green letters
			let answer = "";
			for(let j = 0; j < len; j++){
				board[cursor.y][j].value = board[cursor.y][j].value.toUpperCase();
				let value = board[cursor.y][j].value;
				answer += value;
				numLetters[wordArray[j]].word += 1;
			}

			let validWord=false;

			for(let j = 0; j < dictionary.length; j++){
				if(answer == dictionary[j]){
					validWord=true;
				}
			}

			if(answer == word){
				validWord=true;
				done=true;
				opacity="25";
			}

			if(!validWord){
				cursor.x = 0;
				for(let j = 0; j < len; j++) board[cursor.y][j].value = "";
				return;
			}

			for(let j = 0; j < len; j++){
				let value = board[cursor.y][j].value;
				if(wordArray[j] == value){
					numLetters[value].answer += 1;
					keyColors[value] = c;
					board[cursor.y][j].color = c;
				}
			}

			//Check for yellow letters
			for(let a = 0; a < len; a++){
				if(board[cursor.y][a].color != c){
					let value = board[cursor.y][a].value;
					for(let b = 0; b < len; b++){
						if(wordArray[b] == value && numLetters[value].answer < numLetters[value].word){
							board[cursor.y][a].color = p;
							numLetters[value].answer += 1;
							if(keyColors[value] != c){
								keyColors[value] = p;
							}
						}
					}
				}
			}

			//Set remaining letters gray
			for(let j = 0; j < len; j++){
				if(board[cursor.y][j].color == d){
					board[cursor.y][j].color = i;
					if(keyColors[board[cursor.y][j].value] == dk){
						keyColors[board[cursor.y][j].value] = i;
					}
				}	
			}

			cursor.y += 1;
			cursor.x = 0;

			if(cursor.y >= 5){
				opacity = "25";
				done = true;
			}
		}
	}
</script>

<center class="opacity-{opacity}">

	<!--Make sure tailwind classes passed through props get recognized;-->
	<div hidden class="bg-gray-300 opacity-100">
		<div class="bg-gray-500 border-gray-500 opacity-25"/>
		<div class="bg-green-500 border-green-500"/>
		<div class="bg-amber-400 border-amber-400"/>
	</div>

	{#each board as row}
		<div class ="flex flex-row justify-center">
			{#each row as letter}
				{#if letter.color == d}
					<Letter value={letter.value} color={letter.color}/>
				{:else}
					<Letter value={letter.value} color={letter.color} bg={letter.color}/>
				{/if}
			{/each}
		</div>
	{/each}

	<br>

	<!--Allow physical input-->
	<Keydown
	pauseOnInput
	on:key={(e) => {
		processInput(e.detail);
	}}
	/>

	<!--Create Digital Keyboard-->
	<div class="flex flex-col" style="max-width:472px">
		<div class="flex-nowrap">
			{#each keyboard[0] as key}
				<Key value={key} color={keyColors[key]} width="26.6px" on:input={handleInput}/>
			{/each}
		</div>
		<div class="flex-nowrap">
			{#each keyboard[1] as key}
				<Key value={key} color={keyColors[key]} width="30px" on:input={handleInput}/>	
			{/each}
		</div>
		<div class="flex-nowrap">
			{#each keyboard[2] as key}
				{#if key == "BACK"}
					<Key value={key} color={keyColors[key]} width="42px" on:input={handleInput}/>	
				{:else if key == "ENTER"}
					<Key value={key} color={keyColors[key]} width="50px" on:input={handleInput}/>	
				{:else}
					<Key value={key} color={keyColors[key]} width="25.4px" on:input={handleInput}/>	
				{/if}
			{/each}
		</div>
	</div>

</center>

<Alert show={done} {word}/>

<style lang="postcss" global>
	@tailwind base;
	@tailwind components;
	@tailwind utilities;
</style>