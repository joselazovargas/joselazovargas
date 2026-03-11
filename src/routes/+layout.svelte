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
	/* Remove global flash by disabling root cross-fade */
	:global(::view-transition-old(root)),
	:global(::view-transition-new(root)) {
		animation: none;
		mix-blend-mode: normal;
	}

	/* Morphing elements (remix names) use browser default (smooth move + cross-fade) */
	
	/* Experiment content fades in only after the title reaches destination */
	:global(::view-transition-new(experiment-body)) {
		animation: 400ms cubic-bezier(0, 0, 0.2, 1) 300ms both fade-in;
	}

	@keyframes fade-in {
		from { opacity: 0; }
		to { opacity: 1; }
	}
</style>
