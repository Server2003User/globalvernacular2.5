<script lang="ts">
    
    //@ts-ignore
    import Settingsrow from "/src/lib/components/settingsrow.svelte";

    // takes dispatched data and puts it in to cookies
    function sendSetting(e) {
    
        let cookieString = e.detail.choice + "=" + e.detail.tof;
        document.cookie = cookieString;
    
     }

</script>
    
    <div class="w-full h-[90%] flex flex-col items-center">
    
        <div class="m-5 text-neutral-900 dark:text-white font-bold text-6xl md:text-6xl">
            Settings
        </div>
        <div class="flex flex-col justify-center items-center w-full overflow-scroll">
        
        <Settingsrow SetName="Hard Mode" on:selectedSetting={sendSetting}></Settingsrow>
       
        </div>
    </div>