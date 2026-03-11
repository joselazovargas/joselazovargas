<script lang="ts">
	import { onMount } from 'svelte';
	import Syntax from '$lib/components/Syntax.svelte';

	let url = $state('https://joselazo.vargas');
	let qrUrl = $derived(`https://api.qrserver.com/v1/create-qr-code/?size=250x250&data=${encodeURIComponent(url)}`);
	let copied = $state(false);

	async function pasteFromClipboard() {
		try {
			const text = await navigator.clipboard.readText();
			url = text;
		} catch (err) {
			console.error('Failed to read clipboard', err);
		}
	}

	function copyToClipboard() {
		navigator.clipboard.writeText(url);
		copied = true;
		setTimeout(() => copied = false, 2000);
	}
</script>

<div class="min-h-screen bg-gray-900 text-white font-mono p-4 md:p-12 flex flex-col items-center">
	<div class="w-full max-w-4xl">
		<a href="/" class="text-vscode-purple hover:underline mb-8 inline-block">← cd /home</a>

		<div class="mb-4 flex items-center gap-2 text-lg">
			<Syntax type="purple">export</Syntax> 
			<Syntax type="purple">default</Syntax> 
			<Syntax type="purple">const</Syntax> 
			<span style="view-transition-name: remix-qr-generator;">
				<Syntax type="cyan">QRGenerator</Syntax>
			</span> = () => {' {'}
		</div>

		<div class="pl-4 md:pl-8 border-l border-white/10 ml-2 space-y-8" style="view-transition-name: experiment-body;">
			<div class="w-full max-w-md backdrop-blur-md bg-white/5 border border-white/10 rounded-xl p-8 shadow-2xl">
				<div class="space-y-6">
					<div class="flex flex-col gap-2">
						<label for="url" class="text-vscode-red">const <span class="text-vscode-cyan">targetUrl</span> = </label>
						<div class="relative">
							<input 
								type="text" 
								bind:value={url} 
								placeholder="https://..."
								class="w-full bg-black/40 border border-white/10 rounded-lg p-3 text-vscode-green outline-none focus:border-vscode-purple transition-colors"
							/>
							<button 
								onclick={pasteFromClipboard}
								class="absolute right-2 top-1/2 -translate-y-1/2 bg-white/5 hover:bg-white/10 px-3 py-1 rounded text-sm transition-colors"
								title="Paste from clipboard"
							>
								paste
							</button>
						</div>
					</div>

					<div class="flex justify-center p-4 bg-white rounded-lg">
						{#if url}
							<img src={qrUrl} alt="QR Code" class="w-[200px] h-[200px]" />
						{:else}
							<div class="w-[200px] h-[200px] flex items-center justify-center text-gray-400 italic">
								Waiting for URL...
							</div>
						{/if}
					</div>

					<div class="flex justify-between items-center">
						<button 
							onclick={copyToClipboard}
							class="text-vscode-cyan hover:underline cursor-pointer"
						>
							{copied ? 'Copied!' : 'copy url'}
						</button>
						<Syntax type="gray">/* Powered by goqr.me */</Syntax>
					</div>
				</div>
			</div>
		</div>

		<div class="mt-4 text-lg">
			{'}'}
		</div>
	</div>
</div>
