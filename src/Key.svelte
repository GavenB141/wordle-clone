<script>
    import { createEventDispatcher } from 'svelte';

	const dispatch = createEventDispatcher();

    export let value;
    export let color;
	export let width;

	function sendInput() {
		dispatch('input', {
			input: value
		});
	}
</script>

<button class="border-black bg-{color} rounded py-2 m-0.5 text-sm font-bold" on:click={sendInput} style="width:{width}">
    {value}
</button>