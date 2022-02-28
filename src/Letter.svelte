<script>
    import { scale } from 'svelte/transition';

    export let value = "Q";
    export let color = "gray-300";
    export let bg = "";

    function flipIn(node, { duration = 150}) {
		return {
			duration,
			css: t => `
			transform: rotateY(${90 + (-90) * t}deg)`
		};
	}
</script>

{#key value}
<div in:flipIn class="text-white text-center border-2 m-1 border-{color} bg-{bg} pt-3 font-semibold text-3xl" style="width:60px; height:60px">
    {value}
</div>
{/key}