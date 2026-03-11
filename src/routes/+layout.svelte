<script lang="ts">
	import '../app.css';
	import { onNavigate } from '$app/navigation';
	
	let { children } = $props();

	onNavigate((navigation) => {
		if (!document.startViewTransition) return;

		return new Promise((resolve) => {
			document.startViewTransition(async () => {
				resolve();
				await navigation.complete;
			});
		});
	});
</script>

<div class="min-h-screen bg-gray-900 overflow-x-hidden selection:bg-vscode-purple/30">
	{@render children()}
</div>

<style>
	:global(::view-transition-old(root)),
	:global(::view-transition-new(root)) {
		animation-duration: 0.4s;
		animation-timing-function: cubic-bezier(0.4, 0, 0.2, 1);
	}
</style>
