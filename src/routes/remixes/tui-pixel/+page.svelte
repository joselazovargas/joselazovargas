<script lang="ts">
	import { onMount } from 'svelte';
	import Syntax from '$lib/components/Syntax.svelte';

	let logs = $state<string[]>([]);
	let progress = $state(0);
	let status = $state('Initializing...');
	let isRunning = $state(true);

	const messages = [
		'Booting kernel v4.0.0...',
		'Loading drivers...',
		'Starting network services...',
		'Mounting remote disks...',
		'Synchronizing clock...',
		'Compiling shaders...',
		'Optimizing pixel buffer...',
		'Handshaking with the void...',
		'System Ready.'
	];

	onMount(() => {
		let i = 0;
		const interval = setInterval(() => {
			if (i < messages.length) {
				logs = [...logs, `> ${messages[i]}`];
				progress = Math.min(100, (i + 1) * (100 / messages.length));
				status = messages[i];
				i++;
			} else {
				isRunning = false;
				clearInterval(interval);
			}
		}, 600);
	});
</script>

<div class="min-h-screen bg-black text-[#00ff00] font-mono p-4 md:p-12 flex flex-col items-center">
	<div class="w-full max-w-4xl">
		<a href="/" class="text-[#00ff00] hover:underline mb-8 inline-block">← cd /home</a>
		
		<div class="mb-4 flex items-center gap-2 text-lg">
			<Syntax type="purple">export</Syntax> 
			<Syntax type="purple">default</Syntax> 
			<Syntax type="purple">const</Syntax> 
			<span style="view-transition-name: remix-tui-pixel;">
				<Syntax type="cyan">TUIPixelAnimation</Syntax>
			</span> = () => {' {'}
		</div>

		<div class="pl-4 md:pl-8 border-l border-[#00ff00]/20 ml-2 space-y-8" style="view-transition-name: experiment-body;">
			<div class="w-full max-w-2xl border-2 border-[#00ff00] bg-black p-6 shadow-[0_0_20px_#00ff0033] relative">
				<!-- Scanline effect -->
				<div class="absolute inset-0 pointer-events-none bg-[linear-gradient(rgba(18,16,16,0)_50%,rgba(0,0,0,0.25)_50%),linear-gradient(90deg,rgba(255,0,0,0.06),rgba(0,255,0,0.02),rgba(0,0,255,0.06))] bg-[length:100%_2px,3px_100%] z-10"></div>
				
				<div class="flex justify-between items-center mb-6 border-b border-[#00ff00] pb-2">
					<span class="animate-pulse text-sm">TERMINAL V4.0.0</span>
					<span class="text-xs">STATUS: {status}</span>
				</div>

				<div class="h-64 overflow-y-auto space-y-1 mb-6 scrollbar-hide">
					{#each logs as log}
						<div class="opacity-80 text-sm">{log}</div>
					{/each}
					{#if isRunning}
						<div class="flex items-center gap-2">
							<span class="animate-pulse">></span>
							<span class="h-4 w-2 bg-[#00ff00] animate-blink"></span>
						</div>
					{/if}
				</div>

				<div class="space-y-2">
					<div class="flex justify-between text-xs">
						<span>SYSTEM_LOAD</span>
						<span>{Math.round(progress)}%</span>
					</div>
					<div class="w-full h-4 border border-[#00ff00] p-0.5">
						<div 
							class="h-full bg-[#00ff00] transition-all duration-300"
							style="width: {progress}%"
						></div>
					</div>
				</div>

				{#if !isRunning}
					<div class="mt-8 p-4 border border-dashed border-[#00ff00] text-center">
						<p class="text-xl font-bold animate-pulse">SYSTEM OPERATIONAL</p>
						<button 
							onclick={() => window.location.reload()}
							class="mt-4 border border-[#00ff00] px-4 py-2 hover:bg-[#00ff00] hover:text-black transition-colors"
						>
							REBOOT_
						</button>
					</div>
				{/if}
			</div>
		</div>

		<div class="mt-4 text-lg">
			{'}'}
		</div>
	</div>
</div>

<style>
	@keyframes blink {
		0%, 100% { opacity: 1; }
		50% { opacity: 0; }
	}
	.animate-blink {
		animation: blink 1s step-end infinite;
	}
	.scrollbar-hide::-webkit-scrollbar {
		display: none;
	}
</style>
