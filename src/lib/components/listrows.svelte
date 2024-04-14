<script lang="ts">
    
    export let SetName;

</script>



<div class="bg-neutral-200 dark:bg-neutral-800 h-48 w-1/2 rounded-md m-5 flex flex-col items-start border-2 border-neutral-300 dark:border-neutral-700">
    <div class="text-neutral-900 dark:text-white font-bold text-5xl m-5">
        {SetName || "N/A"}
    </div>
    <a href="/game" class="h-10 w-20 rounded-md text-neutral-900 hover:text-neutral-950 dark:text-white dark:hover:text-neutral-200 bg-green-500 hover:bg-green-600 dark:bg-green-900 dark:hover:bg-green-950 m-5 text-2xl flex justify-center items-center">
        Play
    </a>
</div>