<script>
    import "../app.css";
    import Settingsicon from "../lib/components/settingsicon.svelte";
</script>
  
<div class="global-font h-dvh w-dvw flex flex-col dark:bg-neutral-900 overflow-hidden">
    <div class="w-dvw h-20 bg-gradient-to-r from-emerald-500 to-sky-500 flex justify-center items-center dark:from-emerald-900 dark:to-sky-900">
            
        <div class="flex justify-center items-center mx-5">
            <a href="/" class="font-bold text-5xl text-white hover:text-neutral-200 dark:text-neutral-900 dark:hover:text-neutral-950 w-full h-full">
                GV
            </a>
        </div>

        <div class="flex flex-row justify-end items-center h-full w-full">
                <a href="/settings" class="h-14 w-14 m-5 text-md md:text-2xl flex flex-row items-center justify-center bg-white hover:bg-neutral-200 dark:bg-neutral-900 dark:hover:bg-neutral-950 rounded-md">
                        <Settingsicon/>
                </a>   
            </div>
        </div>
        <div class="h-full w-full">
            <slot/>
        </div>
 </div>
