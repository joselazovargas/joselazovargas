<script lang="ts">
	import CardFace from '$lib/components/CardFace.svelte';
	import { dev } from '$app/environment';

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
	const tokens = [
		`<span class="text-vscode-purple">const</span>`, ' ', `<span class="text-vscode-cyan">aboutMe</span>`, ' ', `= `, '{', '\n  ',
		`<span class="text-vscode-red">name</span>`, ': ', `<span class="text-vscode-green">'Jose Lazo'</span>`, ',', '\n  ',
		`<span class="text-vscode-red">github</span>`, ': ', `<span class="text-vscode-green">'@joselazovargas'</span>`, ',', '\n  ',
		`<span class="text-vscode-red">linkedin</span>`, ': ', `<span class="text-vscode-green">'@jose-lazo-ict'</span>`, ',', '\n  ',
		`<span class="text-vscode-red">email</span>`, ': ', `<span class="text-vscode-green">'[hidden]'</span>`, ',', '\n  ',
		`<span class="text-vscode-red">phone</span>`, ': ', `<span class="text-vscode-green">'[hidden]'</span>`, ',', '\n  ',
		...(dev ? [`<span class="text-vscode-red">myWork</span>`, ': ', `[]`, ',', '\n  '] : []),
		`<span class="text-vscode-red">readMore</span>`, ': ', '{ ', `<span class="text-vscode-red">job</span>`, ': ', `<span class="text-vscode-green">'BLACK6'</span>`, ' }', '\n',
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
		}, 60);

		return () => {
			clearInterval(interval);
		};
	});

	const emailValue = 'jose@black6.com';
	const phoneValue = '+506 8708 0995';
</script>

<svelte:window on:keydown={startTyping} />

<div class="flex items-center justify-center w-full h-[100svh] bg-gray-900 p-5">
	<div class="relative w-[622px] h-[602px] overflow-hidden rounded-lg" style="perspective:600px;">
		<div
			onclick={startTyping}
			onmousemove={handleMouseMove}
			class="card absolute inset-0 cursor-default transition-transform duration-500 ease-out will-change-transform"
		>
			<div class="light"></div>
			<CardFace 
				{stage} 
				{displayedHtml} 
				{isVerified} 
				{statsExpanded} 
				{emailValue} 
				{phoneValue}
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