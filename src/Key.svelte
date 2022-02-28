<script>
    import { createEventDispatcher } from 'svelte';

	const dispatch = createEventDispatcher();

    export let value;
    export let color;

	function sendInput() {
		dispatch('input', {
			input: value
		});
	}
</script>

<button class="border-black bg-{color} rounded py-2 px-3 m-0.5 font-semibold" on:click={sendInput}>
    {value}
</button>