<script lang="ts">
	import CardFace from '$lib/components/CardFace.svelte';

	// Stage 0: waiting, 1: typing, 2: done
	let stage = $state(0);
	let displayedHtml = $state('');

	// Hidden / collapsed states
	let isVerified = $state(false);
	let statsExpanded = $state(false);
	let mouseMoveCount = 0;

	// Simple "invisible robot captcha" check
	function handleMouseMove() {
		if (stage === 2 && !isVerified) {
			mouseMoveCount++;
			if (mouseMoveCount > 50) { // human-like movement threshold
				isVerified = true;
			}
		}
	}

	// Syntax-colored tokens for your contact block
	// email is above readMore, myWork is hidden (WIP)
	const tokens = [
		`<span class="text-vscode-purple">const</span>`, ' ', `<span class="text-vscode-cyan">aboutMe</span>`, ' ', `= `, '{', '\n  ',
		`<span class="text-vscode-red">name</span>`, ': ', `<span class="text-vscode-green">'Jose Lazo'</span>`, ',', '\n  ',
		`<span class="text-vscode-red">github</span>`, ': ', `<a href="https://github.com/joselazovargas" target="_blank" class="text-vscode-green no-underline">'@joselazovargas'</a>`, ',', '\n  ',
		`<span class="text-vscode-red">linkedin</span>`, ': ', `<a href="https://www.linkedin.com/in/jose-lazo-ict/" target="_blank" class="text-vscode-green no-underline">'@jose-lazo-ict'</a>`, ',', '\n  ',
		`<span class="text-vscode-red">email</span>`, ': ', `<span class="text-vscode-green">'[hidden]'</span>`, ',', '\n  ',
		`<span class="text-vscode-red">readMore</span>`, ': ', '{ ', `<span class="text-vscode-red">job</span>`, ': ', `<a href="https://black6.com" target="_blank" class="text-vscode-green no-underline">'BLACK6'</a>`, ' }', '\n',
		'};', '\n\n',
		`<span class="text-vscode-purple">const</span>`, ' ', `<span class="text-vscode-cyan">statsForNerds</span>`, ' ', `= `, '{', ' ... ', '};'
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
		}, 60); // slightly faster

		return () => {
			clearInterval(interval);
		};
	});

	const emailValue = 'jose@black6.com';
</script>

<svelte:window on:keydown={startTyping} />

<div class="flex items-center justify-center w-full h-[100svh] bg-gray-900 p-5">
	<div class="relative w-[422px] h-[302px] overflow-hidden rounded-lg" style="perspective:600px;">
		<div
			onclick={startTyping}
			onmousemove={handleMouseMove}
			class="card absolute inset-0 cursor-default transition-transform duration-500 ease-out will-change-transform"
			style="transform-style: preserve-3d; --light-x:50%; --light-y:50%;"
		>
			<div class="light"></div>
			<CardFace 
				{stage} 
				{displayedHtml} 
				{isVerified} 
				{statsExpanded} 
				{emailValue} 
				onExpandStats={() => statsExpanded = true} 
			/>
		</div>
	</div>
</div>

<style>
	.card {
		transform-style: preserve-3d;
		-webkit-transform-style: preserve-3d;
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
</style>