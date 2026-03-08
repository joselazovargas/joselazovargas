<script lang="ts">
	import Syntax from './Syntax.svelte';

	interface Props {
		name: string;
		value: string;
		isLink?: boolean;
		url?: string;
		isSecret?: boolean;
		isVerified?: boolean;
		actualSecret?: string;
		isLast?: boolean;
		indent?: number;
		canCopy?: boolean;
		onReveal?: () => void;
	}

	let { 
		name, 
		value, 
		isLink = false, 
		url = '', 
		isSecret = false, 
		isVerified = false, 
		actualSecret = '',
		isLast = false,
		indent = 1,
		canCopy = false,
		onReveal
	}: Props = $props();

	let copied = $state(false);

	function copyToClipboard() {
		if (!canCopy || !isVerified) return;
		const textToCopy = isSecret ? actualSecret : (isLink ? url : value);
		navigator.clipboard.writeText(textToCopy);
		copied = true;
		setTimeout(() => copied = false, 2000);
	}
</script>

<div class="group flex items-center gap-2" style="padding-left: {indent * 1}rem;">
	<div class="flex-shrink-0">
		<Syntax type="red">{name}</Syntax>: 
		{#if isLink}
			<a href={url} target="_blank" class="text-vscode-green no-underline hover:underline">'{value}'</a>
		{:else if isSecret}
			{#if isVerified}
				<Syntax type="green" class="transition-all duration-500">'{actualSecret}'</Syntax>
			{:else}
				<button 
					onclick={onReveal}
					class="text-vscode-green italic cursor-pointer hover:bg-white/5 px-1 rounded transition-colors"
				>
					'tap to reveal'
				</button>
			{/if}
		{:else}
			<Syntax type="green">'{value}'</Syntax>
		{/if}{#if !isLast},{/if}
	</div>
	
	{#if canCopy && (!isSecret || isVerified)}
		<button 
			onclick={copyToClipboard}
			class="opacity-0 group-hover:opacity-100 transition-all duration-300 p-1 hover:bg-white/10 rounded cursor-pointer md:opacity-0 touch-device:opacity-100"
			title="Copy to clipboard"
		>
			{#if copied}
				<svg xmlns="http://www.w3.org/2000/svg" width="12" height="12" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" class="text-vscode-green"><polyline points="20 6 9 17 4 12"></polyline></svg>
			{:else}
				<svg xmlns="http://www.w3.org/2000/svg" width="12" height="12" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" class="text-gray-500"><rect x="9" y="9" width="13" height="13" rx="2" ry="2"></rect><path d="M5 15H4a2 2 0 0 1-2-2V4a2 2 0 0 1 2-2h9a2 2 0 0 1 2 2v1"></path></svg>
			{/if}
		</button>
	{/if}
</div>

<style>
	@media (hover: none) {
		button {
			opacity: 1 !important;
		}
	}
</style>