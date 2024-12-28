<script lang="ts">
	import '../app.css';
	let { children } = $props();
</script>

<div class="mx-auto my-4 max-w-2xl rounded-lg bg-white p-4">
	{@render children()}
</div>
