<script lang="ts">
	// Stage 0: waiting, 1: typing, 2: done
	let stage = $state(0);
	let displayedHtml = $state('');

	// Syntax-colored tokens for your contact block
	const tokens = [
		`<span class="text-vscode-purple">const</span>`, ' ', `<span class="text-vscode-cyan">aboutMe</span>`, ' ', `= `, '{', '\n  ',
		`<span class="text-vscode-red">name</span>`, ': ', `<span class="text-vscode-green">'Jose Lazo'</span>`, ',', '\n  ',
		`<span class="text-vscode-red">job</span>`, ': ', `<a href="https://black6.com" target="_blank" class="text-vscode-green no-underline">'BLACK6'</a>`, ',', '\n  ',
		`<span class="text-vscode-red">github</span>`, ': ', `<a href="https://github.com/joselazovargas" target="_blank" class="text-vscode-green no-underline">'@joselazovargas'</a>`, ',', '\n  ',
		`<span class="text-vscode-red">linkedin</span>`, ': ', `<a href="https://www.linkedin.com/in/jose-lazo-ict/" target="_blank" class="text-vscode-green no-underline">'@jose-lazo-ict'</a>`, '\n',
		'}'
	];

	// A single function to start the animation
	function startTyping(e: KeyboardEvent | MouseEvent) {
		if (stage !== 0) return;
		if (e instanceof KeyboardEvent && ['Shift', 'Control', 'Alt', 'Meta'].includes(e.key)) {
			return;
		}
		stage = 1;
	}

	// This side effect runs automatically when `stage` becomes 1
	$effect(() => {
		if (stage !== 1) return;

		let i = 0;
		const interval = setInterval(() => {
			displayedHtml += tokens[i++];
			if (i === tokens.length) {
				clearInterval(interval);
				stage = 2;
			}
		}, 100);

		// Svelte's cleanup function prevents memory leaks
		return () => {
			clearInterval(interval);
		};
	});
</script>

<svelte:window on:keydown={startTyping} />

<div class="flex items-center justify-center w-full h-[100svh] bg-gray-900 p-5">
	<div class="relative w-[422px] h-[302px] overflow-hidden rounded-lg" style="perspective:600px;">
		<div
			on:click={startTyping}
			class="card absolute inset-0 cursor-default transition-transform duration-500 ease-out will-change-transform"
			style="transform-style: preserve-3d; --light-x:50%; --light-y:50%;"
		>
			<div class="light"></div>
			<div class="face absolute inset-0 backdrop-blur-md rounded-md shadow-xl p-4 font-mono text-white overflow-auto whitespace-pre-wrap">
				{#if stage === 0}
					<div class="flex h-full items-center">
						<span class="text-gray-400 text-sm md:text-lg">Press any key to continue...</span>
						<span class="cursor text-gray-400 text-lg ml-2">_</span>
					</div>
				{:else}
					<div>
						{@html displayedHtml}<span class="cursor text-white">_</span>
					</div>
				{/if}
			</div>
		</div>
	</div>
</div>

<style>
	.card {
		transform-style: preserve-3d;
		-webkit-transform-style: preserve-3d;
	}
	.face {
		backface-visibility: hidden;
		-webkit-backface-visibility: hidden;
	}
	.light {
		position: absolute;
		width: 200%;
		height: 200%;
		top: -50%;
		left: -50%;
		pointer-events: none;
		background: radial-gradient(
			circle at var(--light-x) var(--light-y),
			rgba(255, 255, 255, 0.15),
			transparent 80%
		);
		opacity: 0;
		transition: opacity 0.3s ease-out;
	}
	@keyframes blink {
		0%,
		50% {
			opacity: 1;
		}
		50.01%,
		100% {
			opacity: 0;
		}
	}
	.cursor {
		display: inline-block;
		animation: blink 1s step-start infinite;
	}
</style>