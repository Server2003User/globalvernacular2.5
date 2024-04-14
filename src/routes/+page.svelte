<script lang="ts">

    import { onMount } from 'svelte';
    import { countries, getEmojiFlag, getCountryCode } from 'countries-list';
    import lodash from 'lodash';
    // @ts-ignore
    import languageNames from 'countries-list/minimal/languages.native.min';
    // @ts-ignore
    import languageNamesEng from 'countries-list/minimal/languages.en.min';
    
    import type { TCountryCode } from 'countries-list';
    
    function sleep(time) {
      return new Promise((resolve) => setTimeout(resolve, time));
    }
    
    let randomNation1 = lodash.sample(countries);
    let randomNation2 = lodash.sample(countries);
    let randomNation3 = lodash.sample(countries);
    let randomNation4 = lodash.sample(countries);
    
    let nationChoice = lodash.sample([randomNation1, randomNation2, randomNation3, randomNation4]);
    let languageChoice = nationChoice.languages
    let languageName;
    let inputValue;
    let languageNameEng;
    let countryEmoji;
    let falseAnswer;
    let correctScore = 0;
    let falseScore = 0;
    let timeSeconds;
    let timeMinutes;
    let tsString;
    let tmString;
    let timeInterval;
    let totalTimeInterval;
    let ttimeSeconds = 0;
    let ttimeMinutes = 0;
    let ttseString = "00";
    let ttmString = "00";
    let difficulty;
    
    function easyOrHard() {
    
        const storedHard = document.cookie.split("; ").find((row) => row.startsWith("Hard Mode" + "=")).split("=")[1];
        console.log(storedHard);
        if (storedHard == undefined) {
            difficulty = false;
        } else if (storedHard == "false") {
            difficulty = false;
        } else if (storedHard == "true") {
            difficulty = true;
        }
    
    
    }
    
    function timer() {
    
        // Time logic for clock
        timeSeconds += 1;
        if (timeSeconds == 60) {
    
            timeMinutes += 1;
            timeSeconds = 0;
    
        }
    
        // turning that into string for game
        
        if (timeSeconds < 10) {
            tsString = "0" + timeSeconds;
        } else {
            tsString = timeSeconds;
        }
    
        if (timeMinutes < 10) {
            tmString = "0" + timeMinutes;
        } else {
            tmString = timeMinutes;
        }
    
    }
    
    function totalTimer() {
    
    // same as above but for the total time
    
    // Time logic for clock
        ttimeSeconds += 1;
        if (ttimeSeconds == 60) {
    
            ttimeMinutes += 1;
            ttimeSeconds = 0;
    
        }
    
        // turning that into string for game
        
        if (ttimeSeconds < 10) {
            ttseString = "0" + ttimeSeconds;
        } else {
            ttseString = ttimeSeconds;
        }
    
        if (ttimeMinutes < 10) {
            ttmString = "0" + ttimeMinutes;
        } else {
            ttmString = ttimeMinutes;
        }
    
    }
    
    function timerReset() {
    
        // resets all variables for the above function
        timeSeconds = 0;
        timeMinutes = 0;
        tsString = "00";
        tmString = "00";
        clearInterval(timeInterval);
        timeInterval = null;
    
    }
    
    function timerHandling() {
    
        // handles time management
        timerReset();
        timeInterval = setInterval(timer, 1000);
        
    }
    
    function getEmoji(country) {
    
        let countryCode = getCountryCode(country.name);
        
        if (countryCode !== false) {
            countryEmoji = getEmojiFlag(countryCode);
            console.log(countryEmoji); 
        } 
    
    }
    
    function newRound() {
        randomNation1 = lodash.sample(countries);
        randomNation2 = lodash.sample(countries);
        randomNation3 = lodash.sample(countries);
        randomNation4 = lodash.sample(countries);
        falseAnswer = false;
        timerHandling();
        nationChoice = lodash.sample([randomNation1, randomNation2, randomNation3, randomNation4]);
        getEmoji(nationChoice);
        languageChoice = nationChoice.languages
        languageName;
    
        if (languageChoice.length > 1) {
            let languagesForNation = Object.entries(languageChoice);
            languageName = [];
            for (let key = 0; key < languagesForNation.length; key++) {
                languageName[key] = languageNames[languageChoice[key]];
            }
            languageNameEng = [];
            for (let key = 0; key < languagesForNation.length; key++) {
                languageNameEng[key] = languageNamesEng[languageChoice[key]];
            }
            languageName = languageName.toString();
            languageNameEng = languageNameEng.toString();
        } else {
            languageName = languageNames[languageChoice];
            languageNameEng = languageNamesEng[languageChoice];
        }
    
    }
    
    async function questionHandler(choice) {
        let languageChoosed;
        if (choice == 1) {
            multiQuestionHandler(randomNation1.languages)
        } else if (choice == 2) {
            multiQuestionHandler(randomNation2.languages);
        } else if (choice == 3) {
            multiQuestionHandler(randomNation3.languages);
        } else if (choice == 4) {
            multiQuestionHandler(randomNation4.languages);
        } else {
            console.log(choice);
            console.log(nationChoice);
            inputQuestionHandler(choice);
        }
    
    }
    
    async function multiQuestionHandler(language) {
    
        let languagesForNation = Object.entries(language);
        let languageNameArray = [];
    
         for (let i = 0; i < languagesForNation.length; i++) {
            languageNameArray[i] = languageNames[language[i]];
         }
    
         if (languageNameArray == languageName) {
            correctScore += 1;
            newRound();
         } else {
            falseAnswer = true;
            sleep(500).then(() => { falseScore += 1;  newRound();});
         }
    
    }
    
    async function inputQuestionHandler(language) {
    
     if (language == nationChoice.name) {
        correctScore += 1;
        inputValue.value = "";
        newRound();
     } else {
        falseAnswer = true;
        inputValue.value = "";
        sleep(500).then(() => { falseScore += 1;  newRound();});
     }
    
    }
    
    onMount(() => {
		easyOrHard();
	});
    
    newRound();
    totalTimeInterval = setInterval(totalTimer, 1000);
    
    </script>
    
    <div class="h-full w-full grid grid-cols-4 grid-rows-4">
        {#if !difficulty}
        <div class="h-full w-full grid grid-cols-1 grid-rows-4 row-span-4 col-start-1">
            <button on:click={() => questionHandler(1)} class="h-full w-full bg-red-400 hover:bg-red-500 dark:bg-red-800 dark:hover:bg-red-900  text-neutral-900 hover:text-neutral-900/50 dark:text-neutral-200 hover:dark:text-neutral-200/50  text-xl md:text-3xl">
                { randomNation1.name || "N/A"}
            </button>
            <button on:click={() => questionHandler(2)} value="randomNation2" class="h-full w-full bg-green-400 hover:bg-green-500 dark:bg-green-800 dark:hover:bg-green-900  text-neutral-900 hover:text-neutral-900/50 dark:text-neutral-200 hover:dark:text-neutral-200/50  text-xl md:text-3xl">
                { randomNation2.name || "N/A"}
            </button>
            <button on:click={() => questionHandler(3)} value="randomNation3" class="h-full w-full bg-cyan-400 hover:bg-cyan-500 dark:bg-cyan-800 dark:hover:bg-cyan-900 text-neutral-900 hover:text-neutral-900/50 dark:text-neutral-200 hover:dark:text-neutral-200/50  text-xl md:text-3xl">
                { randomNation3.name || "N/A"}
            </button>
            <button on:click={() => questionHandler(4)} value="randomNation4" class="h-full w-full bg-yellow-400 hover:bg-yellow-500 dark:bg-yellow-600 dark:hover:bg-yellow-700 text-neutral-900 hover:text-neutral-900/50 dark:text-neutral-200 hover:dark:text-neutral-200/50  text-xl md:text-3xl">
                { randomNation4.name || "N/A"}
            </button>
        </div>
        <div class="h-full w=[50%] row-span-4 col-start-2 col-span-3 flex flex-col justify-start items-center">
            <div class="h-[10%] w-full mx-1 text-md md:text-2xl flex flex-row items-center justify-center  bg-neutral-200 dark:bg-neutral-800">
                <div class="text-green-900 dark:text-green-300 m-1">
                    Right: {correctScore}
                </div>
                <div class="text-red-800 dark:text-red-300 m-1">
                    Wrong: {falseScore}
                </div>
                <div class="text-neutral-800 dark:text-neutral-300 m-1">
                    Time: {tmString + ":" + tsString}
                </div>
                <div class="text-neutral-800 dark:text-neutral-300 m-1">
                    Total Time: {ttmString + ":" + ttseString}
                </div>
            </div>
            <div class="h-[90%] w-full flex justify-center items-center flex-col">
            {#if falseAnswer == false}
                {#if languageName != languageNameEng}
                    <div class="h-fit w-fit text-3xl md:text-5xl text-neutral-900 dark:text-neutral-200 m-1 md:m-2">
                        {"Native: " + languageName}
                    </div>
                    <div class="h-fit w-fit text-3xl md:text-5xl text-neutral-900 dark:text-neutral-200 m-1 md:m-2">
                        {"English: " + languageNameEng}
                    </div>
                {/if}
                {#if languageName == languageNameEng}
                    <div class="h-fit w-fit text-3xl md:text-5xl text-neutral-900 dark:text-neutral-200 m-1 md:m-2">
                        {languageNameEng}
                    </div>
                {/if}
            {/if}
            {#if falseAnswer == true}
                <div class="h-fit w-fit text-3xl md:text-5xl text-red-900 dark:text-red-400 m-1 md:m-2 blink">
                    {nationChoice.name}
                </div>
            {/if}
            </div>
        </div>
        {/if}
        {#if difficulty}
        <div class="h-full w-full row-span-4 col-start-1 col-span-4 flex flex-col justify-center items-center">
            <div class="h-[10%] w-full mx-1 text-md md:text-2xl flex flex-row items-center justify-center  bg-neutral-200 dark:bg-neutral-800">
                <div class="text-green-900 dark:text-green-300 m-1">
                    Right: {correctScore}
                </div>
                <div class="text-red-800 dark:text-red-300 m-1">
                    Wrong: {falseScore}
                </div>
                <div class="text-neutral-800 dark:text-neutral-300 m-1">
                    Time: {tmString + ":" + tsString}
                </div>
                <div class="text-neutral-800 dark:text-neutral-300 m-1">
                    Total Time: {ttmString + ":" + ttseString}
                </div>
            </div>
            <div class="h-[90%] w-full flex justify-center items-center flex-col">
            {#if falseAnswer == false}
                {#if languageName != languageNameEng}
                    <div class="h-fit w-fit text-3xl md:text-5xl text-neutral-900 dark:text-neutral-200 m-1 md:m-2">
                        {"Native: " + languageName}
                    </div>
                    <div class="h-fit w-fit text-3xl md:text-5xl text-neutral-900 dark:text-neutral-200 m-1 md:m-2">
                        {"English: " + languageNameEng}
                    </div>
                {/if}
                {#if languageName == languageNameEng}
                    <div class="h-fit w-fit text-3xl md:text-5xl text-neutral-900 dark:text-neutral-200 m-1 md:m-2">
                        {languageNameEng}
                    </div>
                {/if}
                <div class="h-[10%] w-1/2 md:w-1/4 bg-neutral-200 dark:bg-neutral-800 text-neutral-900 dark:text-neutral-200 text-4xl rounded-md m-3">
                    <input class="bg-transparent h-full w-full active:border-2 active-border-neutral-700" bind:this={inputValue}>
                </div>
                <button class="h-[5%] w-1/6 bg-green-200 dark:bg-green-800 rounded-md m-3 text-neutral-900 dark:text-neutral-200 text-2xl" on:click={() => questionHandler(inputValue.value)}>
                    Guess
                </button>
            {/if}
            {#if falseAnswer == true}
                <div class="h-fit w-fit text-3xl md:text-5xl text-red-900 dark:text-red-400 m-1 md:m-2 blink">
                    {nationChoice.name}
                </div>
            {/if}
            </div>
        </div>
        {/if}
    </div>