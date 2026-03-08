<script lang="ts">
	import Syntax from './Syntax.svelte';

	interface Props {
		name: string;
		value: string;
		isLink?: boolean;
		url?: string;
		isLast?: boolean;
		indent?: number;
		canCopy?: boolean;
		type?: 'email' | 'phone' | 'default';
	}

	let { 
		name, 
		value, 
		isLink = false, 
		url = '', 
		isLast = false,
		indent = 1,
		canCopy = false,
		type = 'default'
	}: Props = $props();

	let copied = $state(false);
	let showOptions = $state(false);

	function copyToClipboard(e: MouseEvent) {
		e.stopPropagation();
		if (!canCopy) return;
		const textToCopy = isLink ? url : value;
		navigator.clipboard.writeText(textToCopy);
		copied = true;
		setTimeout(() => copied = false, 2000);
	}

	function handleAction(action: string) {
		if (action === 'email') {
			window.location.href = `mailto:${value}`;
		} else if (action === 'call') {
			window.location.href = `tel:${value.replace(/\s/g, '')}`;
		} else if (action === 'whatsapp') {
			const cleanPhone = value.replace(/[^\d]/g, '');
			window.open(`https://wa.me/${cleanPhone}`, '_blank');
		}
		showOptions = false;
	}
</script>

<div class="group flex items-center gap-2" style="padding-left: {indent * 1}rem;">
	<div class="flex-shrink-0 flex items-center">
		<Syntax type="red">{name}</Syntax>: 
		
		{#if showOptions}
			<div class="flex items-center gap-1 ml-1 text-lg">
				<Syntax type="gray">[</Syntax>
				{#if type === 'email'}
					<button onclick={() => handleAction('email')} class="text-vscode-cyan hover:underline cursor-pointer">send email?</button>
				{:else if type === 'phone'}
					<button onclick={() => handleAction('call')} class="text-vscode-cyan hover:underline cursor-pointer">call</button>
					<Syntax type="gray">|</Syntax>
					<button onclick={() => handleAction('whatsapp')} class="text-vscode-cyan hover:underline cursor-pointer">whatsapp</button>
				{/if}
				<Syntax type="gray">|</Syntax>
				<button onclick={() => showOptions = false} class="text-vscode-red hover:underline cursor-pointer">cancel</button>
				<Syntax type="gray">]</Syntax>
			</div>
		{:else}
			<button 
				onclick={() => type !== 'default' ? showOptions = true : null}
				class="{type !== 'default' ? 'cursor-pointer hover:bg-white/5 rounded px-1 transition-colors' : ''}"
			>
				{#if isLink}
					<a href={url} target="_blank" onclick={(e) => e.stopPropagation()} class="text-vscode-green no-underline hover:underline">'{value}'</a>
				{:else}
					<Syntax type="green">'{value}'</Syntax>
				{/if}
			</button>
		{/if}
		
		{#if !isLast && !showOptions}<Syntax type="white">,</Syntax>{/if}
	</div>
	
	{#if canCopy && !showOptions}
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