<script lang="ts">
	let className;

	// noinspection JSUnusedGlobalSymbols
	export { className as class };
</script>

<div class="container mx-auto px-2 {className}">
	<slot />
</div>
