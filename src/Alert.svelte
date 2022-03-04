<script>
    export let show;
    export let word;
</script>

{#if show}
    <div class = "fixed rounded drop-shadow p-2 text-white bg-slate-500" style="width:310px; top:25%; left:50%; margin-left:-155px">
        <center>
            <p class="text-gray-300">Answer:</p>
            <p class="font-bold text-lg">{word}</p>
        </center>
    </div>
{/if}