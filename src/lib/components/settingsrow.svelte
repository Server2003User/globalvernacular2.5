<script lang="ts">
    
    import { onMount } from 'svelte';
    
    // exports the name of the setting
    export let SetName;

    // initializes variables for checkbox and the cookie setting
    let check;
    let setting = undefined;
    
    import { createEventDispatcher } from "svelte";

    const dispatch = createEventDispatcher();

    // dispatches the setting with the setting name and whether it is true or false to the main settings
    export const selectSetting = (SetName, bool) => {

        dispatch("selectedSetting", {
            choice: SetName,
            tof: bool
        });
    
    }

    onMount(() => {
        // checks whether theres a cookie and checks if its 
        if (document.cookie != undefined) {
            setting = document.cookie.split("; ").find((row) => row.startsWith(SetName + "="));
        }

        // checkbox handling; only sets the checkbox true when explicitly the setting is true
		if (setting == undefined) {
            check = false;
        } else if (setting == SetName + "=false") {
            check = false;
        } else {
            check = true;
        }   
	});

    // actively waits for the check to change for it to do the dispatch function
    $: {
        
        if (check) {
            
            selectSetting(SetName, true);
            
        }
        if (!check) {

            selectSetting(SetName, false);

        }
    }


</script>

<div class="bg-neutral-200 dark:bg-neutral-800 border-2 dark:border-neutral-700 h-6/8 w-1/2 rounded-md m-2 flex flex-row items-center justify-start">
    <div class="text-neutral-900 dark:text-white font-bold md:text-4xl text-xl m-5">
        {SetName}
    </div>
    <label class="switch flex">
        <input type="checkbox" bind:checked={check}>
        <span class="slider"></span>
      </label>
</div>
