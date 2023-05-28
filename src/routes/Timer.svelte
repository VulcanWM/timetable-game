<script>
	import { onDestroy } from 'svelte';

	export let callback;
	export let interval = 1000;
	
	const timer = setInterval(callback, interval);
	onDestroy(() => {
		clearInterval(timer);
	});

</script>
