<script lang="ts">
	import CardFace from '$lib/components/CardFace.svelte';
	import { dev } from '$app/environment';
	import { onMount } from 'svelte';
	import gsap from 'gsap';
	import { TextPlugin } from 'gsap/dist/TextPlugin';

	if (typeof window !== 'undefined') {
		gsap.registerPlugin(TextPlugin);
	}

	// Stage 0: waiting, 1: typing, 2: done
	let stage = $state(0);
	let displayedHtml = $state('');

	// Hidden / collapsed states
	let isVerified = $state(false);
	let statsExpanded = $state(false);
	
	// Stats for Nerds data
	let startTime = 0;
	let renderTime = $state(0);

	onMount(() => {
		if (localStorage.getItem('typing_animation_done')) {
			stage = 2;
			isVerified = true;
		}
	});

	// Unified configuration to avoid duplication
	const contactInfo = {
		name: 'Jose Lazo',
		github: '@joselazovargas',
		githubUrl: 'https://github.com/joselazovargas',
		linkedin: '@jose-lazo-ict',
		linkedinUrl: 'https://www.linkedin.com/in/jose-lazo-ict/',
		email: 'jose.lazo.vargas@gmail.com',
		phone: '+61 0491 620025',
		job: 'BLACK6',
		jobUrl: 'https://black6.com'
	};

	const remixes = [
		{ name: 'tripWebglShader()', label: 'Trip Web GL Shader', url: '/remixes/webgl-techno' },
		{ name: 'neonGridMatrix()', label: 'Neon Grid Matrix', url: '/remixes/neon-grid' },
		{ name: 'tuiPixelAnimation()', label: 'TUI Pixel Animation', url: '/remixes/tui-pixel' },
		{ name: 'qrGenerator()', label: 'QR Generator', url: '/remixes/qr-generator' }
	];

	// Invisible "No User Interaction" Captcha
	function verifyHuman() {
		if (!isVerified) {
			isVerified = true;
		}
	}

	// Generate animation tokens from the config
	const codeLines = [
		{ type: 'purple', text: 'const' }, { type: 'white', text: ' ' }, { type: 'cyan', text: 'aboutMe' }, { type: 'white', text: ' = {' }, { type: 'white', text: '\n  ' },
		{ type: 'red', text: 'name' }, { type: 'white', text: ': ' }, { type: 'green', text: `'${contactInfo.name}'` }, { type: 'white', text: ',' }, { type: 'white', text: '\n  ' },
		{ type: 'red', text: 'github' }, { type: 'white', text: ': ' }, { type: 'green', text: `'${contactInfo.github}'` }, { type: 'white', text: ',' }, { type: 'white', text: '\n  ' },
		{ type: 'red', text: 'linkedin' }, { type: 'white', text: ': ' }, { type: 'green', text: `'${contactInfo.linkedin}'` }, { type: 'white', text: ',' }, { type: 'white', text: '\n  ' },
		{ type: 'red', text: 'email' }, { type: 'white', text: ': ' }, { type: 'green', text: `'${contactInfo.email}'` }, { type: 'white', text: ',' }, { type: 'white', text: '\n  ' },
		{ type: 'red', text: 'phone' }, { type: 'white', text: ': ' }, { type: 'green', text: `'${contactInfo.phone}'` }, { type: 'white', text: ',' }, { type: 'white', text: '\n  ' },
		{ type: 'red', text: 'remixes' }, { type: 'white', text: ': [' },
		...remixes.flatMap((r, i) => [
			{ type: 'white', text: '\n    ' },
			{ type: 'green', text: `'${r.name}'` },
			...(i < remixes.length - 1 ? [{ type: 'white', text: ', ' }] : [])
		]),
		{ type: 'white', text: '\n  ' }, { type: 'white', text: ']' }, { type: 'white', text: ',' }, { type: 'white', text: '\n  ' },
		{ type: 'red', text: 'readMore' }, { type: 'white', text: ': { ' }, { type: 'red', text: 'job' }, { type: 'white', text: ': ' }, { type: 'green', text: `'${contactInfo.job}'` }, { type: 'white', text: ' }' }, { type: 'white', text: '\n' },
		{ type: 'white', text: '};' }, { type: 'white', text: '\n\n' },
		{ type: 'purple', text: 'const' }, { type: 'white', text: ' ' }, { type: 'cyan', text: 'statsForNerds' }, { type: 'white', text: ' = { ... };' }
	];

	const colorMap: Record<string, string> = {
		purple: 'text-vscode-purple',
		cyan: 'text-vscode-cyan',
		red: 'text-vscode-red',
		green: 'text-vscode-green',
		white: 'text-white'
	};

	function startTyping(e: KeyboardEvent | MouseEvent) {
		if (stage !== 0) return;
		if (e instanceof KeyboardEvent && ['Shift', 'Control', 'Alt', 'Meta'].includes(e.key)) {
			return;
		}
		startTime = performance.now();
		stage = 1;
		runGsapAnimation();
	}

	function runGsapAnimation() {
		const tl = gsap.timeline({
			onComplete: () => {
				renderTime = Math.round(performance.now() - startTime);
				stage = 2;
				localStorage.setItem('typing_animation_done', 'true');
				setTimeout(verifyHuman, 500);
			}
		});

		let currentHtml = '';
		codeLines.forEach((line) => {
			const spanClass = colorMap[line.type] || 'text-white';
			const chars = line.text.split('');
			
			chars.forEach((char) => {
				tl.to({}, {
					duration: 0.02,
					onStart: () => {
						const escapedChar = char === '\n' ? '\n' : (char === ' ' ? ' ' : char);
						currentHtml += `<span class="${spanClass}">${escapedChar}</span>`;
						displayedHtml = currentHtml;
					}
				});
			});
		});
	}
</script>

<svelte:window on:keydown={startTyping} on:mousemove={verifyHuman} on:touchstart={verifyHuman} />

<div class="flex items-center justify-center min-h-[100svh] bg-gray-900 p-6">
	<div
		onclick={startTyping}
		class="relative w-full max-w-lg transition-all duration-500 ease-out"
	>
		<div class="backdrop-blur-md bg-white/5 border border-white/10 rounded-xl shadow-2xl p-6 font-mono text-lg text-white overflow-hidden">
			<CardFace 
				{stage} 
				{displayedHtml} 
				{isVerified} 
				{statsExpanded} 
				{contactInfo}
				{remixes}
				{renderTime}
				onExpandStats={() => statsExpanded = true} 
			/>
		</div>
	</div>
</div>