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
		indent?: boolean;
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
		indent = true
	}: Props = $props();
</script>

<span>{#if indent}{'  '}{/if}<Syntax type="red">{name}</Syntax>: {#if isLink}<a href={url} target="_blank" class="text-vscode-green no-underline hover:underline">'{value}'</a>{:else if isSecret}<Syntax type="green">'{isVerified ? actualSecret : value}'</Syntax>{:else}<Syntax type="green">'{value}'</Syntax>{/if}{#if !isLast},{/if}{'\n'}</span>