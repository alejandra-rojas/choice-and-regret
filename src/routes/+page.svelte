
<script lang="ts">
	import { accordion } from './instructions'
	import { innitialWordList } from './wordList'; 
	let isOpen = false;
	let showAppContainer = false;
	let userInput = "";
	let wordList: string[] = innitialWordList;

	console.log('Initial set of words:', wordList.length)

	let randomWords: string[] = [];
	let sessionWordSet: string[][] = [];
	let poemWords: string[] = [];
	let remainingRegrets = 3;

  
	function toggleAccordion() {
    isOpen = !isOpen;
  }

  function closeAccordion() {
    isOpen = false;
  }

	function addNewWords(){
		const newWords = userInput.split(/\s+/).filter(word => word.trim() !== "");
		wordList = [...wordList, ...newWords];
		console.log('Number of words after clicking start:', wordList.length)
		userInput = "";
		selectRandomWords();
		showAppContainer = true; 
	}

	function selectRandomWords() {
	  const randomIndexes: number[] = [];
	  while (randomIndexes.length < 7) {
		const randomIndex: number = Math.floor(Math.random() * wordList.length);
		if (!randomIndexes.includes(randomIndex)) {
		  randomIndexes.push(randomIndex);
		}
	  }
	  randomWords = randomIndexes.map((index) => wordList[index]);
	  sessionWordSet.push([...randomWords]);
	  console.log('Session word set:', sessionWordSet)

	}
  
	function addToPoem(word: any) {
	  poemWords = [...poemWords, word];
	  selectRandomWords();
	  console.log('Selected words:', poemWords)

	}
  
	function regret() {
	  if (remainingRegrets > 0 && confirm("Do you regret your chosen word?")) {
		sessionWordSet.pop();
		poemWords.pop();
		poemWords = poemWords;
		randomWords = [...sessionWordSet[sessionWordSet.length - 1]];
		remainingRegrets -= 1;

	  }
	}

	let selectedWordIndex: number | null = null;

	let modifiedWords: string[] = [];

function selectWord(index: number) {
  selectedWordIndex = selectedWordIndex === index ? null : index;
}

function capitalizeWord(index: number) {
  if (!modifiedWords[index]) {
    modifiedWords[index] = poemWords[index];
  }
  poemWords[index] = poemWords[index].charAt(0).toUpperCase() + poemWords[index].slice(1);
}

function addComma(index: number) {
  if (!modifiedWords[index]) {
    modifiedWords[index] = poemWords[index];
  }
  poemWords[index] += ",";
}
function addSemicolon(index: number) {
  if (!modifiedWords[index]) {
    modifiedWords[index] = poemWords[index];
  }
  poemWords[index] += ";";
}
function addQuestion(index: number) {
  if (!modifiedWords[index]) {
    modifiedWords[index] = poemWords[index];
  }
  poemWords[index] += "?";
}
function addDash(index: number) {
  if (!modifiedWords[index]) {
    modifiedWords[index] = poemWords[index];
  }
  poemWords[index] += "-";
}

function addStar(index: number) {
  if (!modifiedWords[index]) {
    modifiedWords[index] = poemWords[index];
  }
  poemWords[index] += "*";
}

function addPoint(index: number) {
  if (!modifiedWords[index]) {
    modifiedWords[index] = poemWords[index];
  }
  poemWords[index] += ".";
}
function addParentheses(index: number) {
  if (!modifiedWords[index]) {
    modifiedWords[index] = poemWords[index];
  }
  poemWords[index] = '(' + poemWords[index] + ')';
}
function addQuotation(index: number) {
  if (!modifiedWords[index]) {
    modifiedWords[index] = poemWords[index];
  }
  poemWords[index] = '"' + poemWords[index] + '"';
}
function addItalics(index: number) {
  if (!modifiedWords[index]) {
    modifiedWords[index] = poemWords[index];
  }
  poemWords[index] = '<em class="italic">' + poemWords[index] + '</em>';
}

function addLinebreak(index: number) {
  if (!modifiedWords[index]) {
    modifiedWords[index] = poemWords[index];
  }
  poemWords[index] += '<br>';
}

function restore(index: number) {
  if (modifiedWords[index]) {
    poemWords[index] = modifiedWords[index];
    modifiedWords[index] = "";
  }
}

  </script>


<svelte:head>
	<title>Regrets</title>
	<meta name="description" content="A Wordle clone written in SvelteKit" />
</svelte:head>

<h1 class="visually-hidden">Regret</h1>


<div class="regrets-container">

	<div class="instructions">
		<a class="instructions-btn" on:click={toggleAccordion}> {#if isOpen}
			<svg class="w-6 h-6 text-gray-800 dark:text-white" aria-hidden="true" xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24">
				<path stroke="currentColor" stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="m15 9-6 6m0-6 6 6m6-3a9 9 0 1 1-18 0 9 9 0 0 1 18 0Z"/>
			  </svg>
		  {:else}
		  <svg class="w-6 h-6 text-gray-800 dark:text-white" aria-hidden="true" xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24">
			<path stroke="currentColor" stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M10 11h2v5m-2 0h4m-2.6-8.5h0M21 12a9 9 0 1 1-18 0 9 9 0 0 1 18 0Z"/>
		  </svg>
		  {/if} Instruktioner </a>
	
		<div class="accordion" use:accordion={isOpen}>
		  <div class="instruction-text">
			<div><span class="bold">Välkommen att skapa en text ur ånger.</span> Skapandet sker i flera steg. 
			</div>
			<div><span class='orange'>Steg 1</span> har redan skett; 4 personer har skrivit text om ånger. Dessa texter har sedan rensats på skiljetecken och bildat en samling ord.
			</div>
			<div><span class='orange'>Steg 2</span> är att du skriver ord i rutan med rubriken Jag ångrar / jeg fortryder:. Dina ord läggs då till i den befintliga samlingen.
			</div>
			<div><span class='orange'>Steg 3</span> är att du väljer ord som i slutet bildar en ordföljd. Orden genereras slumpmässigt från samlingen ord, som du har varit med och skapat. Du börjar med att välja 1 av de 7 ord som visas på skärmen. Varje valt ord genererar i sin tur slumpmässigt 7 nya ord. Du fortsätter på samma sätt tills du i slutändan har en ordföljd på 13 ord. Under skapandets gång kan du ångra dig max 3 gånger, och då gå tillbaka ett steg för chansen att välja ett annat ord.
			</div>
			<div><span class='orange'>Steg 4</span> är att du redigerar ordföljden. Det du kan ändra i ordföljden är att (1) lägga till versaler, (2) lägga till skiljetecken, (3) kursivera, (4) lägga till radbrytning.
			</div>
			<div><span class='orange'>Steg 5</span> är att du kan välja att kopiera den skapade texten och spara den hos dig, eller lämna sidan och låta den försvinna. När du lämnar sidan försvinner även de eventuella ord du la till i samlingen.
			</div>
		  </div>
		</div>
	</div>


	{#if !showAppContainer}
		<div class="step-one">
			<label for="inputField">Jag ångrar/ jeg fortryder:</label>
  			<textarea on:click={closeAccordion}  class="text-area" rows="6" id="inputField" bind:value={userInput} />
			
 			<button on:click={addNewWords}>start</button>
		</div>
	{/if}
 

	{#if showAppContainer}
	<div class="app-container">
		{#if poemWords.length < 13}
			<div class="regret-container">	
				<div class="words-container">
				
					{#each randomWords as word}
						<button on:click={() => addToPoem(word)} class='word'>{word}</button>
					{/each}

				</div>
				{#if remainingRegrets > 0 && poemWords.length >= 1}
				  <button on:click={regret} class="regret-btn">Regrets?</button>
				{/if}
			</div>
		
				
				{#if poemWords.length >= 1}
				<div class="selection-list">
					{#each poemWords as word}
					<div class="selected-word">{word}</div>
					{/each}
				</div>	
				{/if}	
		{/if}

	</div>	

	<div class="step-three">
		{#if poemWords.length === 13}
			<div class="selection-container">
				{#each poemWords as word, index}
				  {#if selectedWordIndex === index}
				    <div
				      role="button"
				      aria-pressed={selectedWordIndex === index}
				      class="selection selected"
				      on:click={() => selectWord(index)}
				    >
				  {@html word}
				    </div>
				  {:else}
				    <div
				      role="button"
				      aria-pressed={selectedWordIndex === index}
				      class="selection"
				      on:click={() => selectWord(index)}
				    >
				  {@html word}
				    </div>
				  {/if}
				{/each}
			</div>
		{/if}

		{#if selectedWordIndex !== null}
			<div class="formatting-options">
			  <button on:click={() => capitalizeWord(selectedWordIndex)}>C</button>
			  <button on:click={() => addPoint(selectedWordIndex)}>.</button>
			  <button on:click={() => addComma(selectedWordIndex)}>,</button>
			  <button on:click={() => addSemicolon(selectedWordIndex)}>;</button>
			  <button on:click={() => addQuestion(selectedWordIndex)}>?</button>
			  <button on:click={() => addDash(selectedWordIndex)}>-</button>
			  <button on:click={() => addStar(selectedWordIndex)}>*</button>
			  <button on:click={() => addParentheses(selectedWordIndex)}>( )</button>
			  <button on:click={() => addQuotation(selectedWordIndex)}>" "</button>
			  <button on:click={() => addItalics(selectedWordIndex)}><span class="italic">i</span></button> 
			  <button on:click={() => restore(selectedWordIndex)}>RESET</button>
    		  <button on:click={() => addLinebreak(selectedWordIndex)}>ENTER</button>  
			</div>
  		{/if}
	</div>

		

	{/if}
</div>
	



	

	






<style>

	.regrets-container{
		height: 100%;
		width: 100%;
		display: flex;
		flex-direction: column;
		align-items: center;
		justify-items: center;
		flex: 1;
		gap: 1rem;
	}

	.instructions{
		display: flex;
        flex-direction: column;
        align-items: center;
        justify-items: center;
        width: 100%;
		max-width: 750px;
	}

	.instructions-btn{
		display: flex;
		align-items: center;
		color: black;
		font-weight: 600;
	}

	.instructions-btn svg{
		height: 25px;
	}

	.accordion {
		margin: 0.25rem 2rem;
		width: 100%;
		height: fit-content;
		background-color: rgba(255,255,255,0.5) ;
	}

	.instruction-text{
		font-size: 0.9rem;
		line-height: 120%;
		padding: 1rem 1rem 1.5rem;
		display: flex;
		flex-direction: column;
		gap: 0.75rem;
	}

	.bold{
		font-weight: 600;
		font-size: 1rem;
	}

	.orange{
		font-weight: 600;
	}

	.step-one{
		width: 100%;
		display: flex;
		flex-direction: column;
		gap: 1rem;
		align-items: center;
		font-size: 1.5rem;
	}

	.text-area {
		border: none;
		width: 90%;
		max-width: 600px;
		padding: 1rem;
		font-size: 1.5rem;
	}

	.step-one button{
		border: 0.5px solid gainsboro;
		padding:  0.5rem 1rem;
		color: gray;
	}

	.step-one button:hover{
		border: 0.5px solid black;
		color: black;
	}

	

	.app-container {
		width: 100%;
		display: grid;
		grid-template-columns: 4fr 1fr;
		gap: 2rem;		
	}

	.regret-container{
		display: flex;
		flex-direction: column;
	margin-top: 4rem;	}

	.words-container{		
		padding: 2rem 0.5rem;
		display: flex;
		flex-wrap: wrap;
		justify-content: center;
		text-align: center;
		gap: 0.5rem 1rem;
	}

	.regret-btn { 
		width: fit-content;
		display: flex;
		align-items: center;
		justify-content: center;
		text-align: center;
		box-sizing: border-box;
		text-transform: lowercase;
		border-radius: 2px;
		background: #EFEFEF;
		margin: 0 auto;
		border: 0.5px solid gainsboro;
		padding:  0.5rem 1rem;
		color: rgba(0, 0, 0, 0.5);;
	}

	.regret-btn:hover{
        border: 0.5px solid red;
        color: red;
    }

	.selection-list{
		margin: 5% 0;
		display: flex;
		flex-direction: column;
		gap: 0.1rem;
	}

	.word, .selected-word {
		width: fit-content;
		display: flex;
		align-items: center;
		justify-content: center;
		text-align: center;
		box-sizing: border-box;
		text-transform: lowercase;
		border: none;
		border-radius: 2px;
		background: white;
		padding: 0.5rem;
		color: rgba(0, 0, 0, 0.85);
	}

	.selected-word, .word:hover{
		color: black;
	}


	

 .selection-container{
	background-color: white;
	padding: 3rem 4rem;
	min-width: 200px;
 }

	.selection{
		cursor: pointer;
		width: fit-content;
		display: inline;
		text-align: center;
		box-sizing: border-box;
		border: none;
		border-radius: 2px;
		color: rgba(0, 0, 0, 0.7);
		z-index: 100;
		line-height:225%;
	}

	.selected {
	padding: 0.25rem;
    outline: 0.5px solid orangered;
	z-index: 100;
}


	
   .formatting-options{
		display: flex;
		flex-wrap: wrap;
		align-items: center;
		justify-content: center;
		box-sizing: border-box;
		margin-top: 2rem;
		gap: 0.2rem;
	}

	.formatting-options button {
		font-size: 0.8rem;
		width: fit-content;
		display: flex;
		align-items: center;
		justify-content: center;
		text-align: center;
		box-sizing: border-box;
		text-transform: uppercase;
		border-radius: 2px;
		border: 0.5px solid gainsboro;
		padding:  0.5rem 1rem;
		color: rgba(0, 0, 0, 0.9);;
	}

	.italic{
		font-style: italic !important;
	}

	.formatting-options button .italic{
		text-transform: lowercase !important; 
		font-style: italic !important;
	}

	.formatting-options button:hover{
		border-color: black;
	}

</style>
