<script lang="ts">
	import Syntax from './Syntax.svelte';
	import CodeProperty from './CodeProperty.svelte';
	import { dev } from '$app/environment';

	interface Props {
		stage: number;
		displayedHtml: string;
		isVerified: boolean;
		statsExpanded: boolean;
		emailValue: string;
		onExpandStats: () => void;
	}

	let { stage, displayedHtml, isVerified, statsExpanded, emailValue, onExpandStats }: Props = $props();
</script>

<div class="face absolute inset-0 backdrop-blur-md rounded-md shadow-xl p-4 font-mono text-sm text-white overflow-auto whitespace-pre-wrap">
{#if stage === 0}
<div class="flex h-full items-center justify-center"><Syntax type="gray">Press any key to continue...</Syntax><span class="cursor text-gray-400 ml-2">_</span></div>
{:else if stage === 1}
<div>{@html displayedHtml}<span class="cursor text-white">_</span></div>
{:else}
<div><Syntax type="purple">const</Syntax> <Syntax type="cyan">aboutMe</Syntax> = {'{'}{'\n'}<CodeProperty name="name" value="Jose Lazo" /><CodeProperty name="github" value="@joselazovargas" isLink url="https://github.com/joselazovargas" /><CodeProperty name="linkedin" value="@jose-lazo-ict" isLink url="https://www.linkedin.com/in/jose-lazo-ict/" /><CodeProperty name="email" value="[hidden]" isSecret {isVerified} actualSecret={emailValue} />{#if dev}<CodeProperty name="myWork" value="[]" />{/if}{'  '}<Syntax type="red">readMore</Syntax>: {'{ '}<CodeProperty name="job" value="BLACK6" isLink url="https://black6.com" indent={false} isLast />{' }'}{'\n'}{'};'}{'\n\n'}<Syntax type="purple">const</Syntax> <Syntax type="cyan">statsForNerds</Syntax> = {'{ '}
{#if !statsExpanded}
<button onclick={onExpandStats} class="text-gray-500 cursor-pointer hover:text-white transition-colors italic">... /* click to expand */</button>
{:else}
{'\n'}<CodeProperty name="coffee" value="infinity" /><CodeProperty name="os" value="win32" /><CodeProperty name="status" value="building" isLast />
{/if}
{'};'}<span class="cursor text-white">_</span></div>
{/if}
</div>

<style>
	.face {
		backface-visibility: hidden;
		-webkit-backface-visibility: hidden;
	}
	@keyframes blink {
		0%, 50% { opacity: 1; }
		50.01%, 100% { opacity: 0; }
	}
	.cursor {
		display: inline-block;
		animation: blink 1s step-start infinite;
	}
</style>