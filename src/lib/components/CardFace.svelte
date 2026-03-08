<script lang="ts">
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
<div class="flex h-full items-center justify-center">
<span class="text-gray-400">Press any key to continue...</span>
<span class="cursor text-gray-400 ml-2">_</span>
</div>
{:else if stage === 1}
<div>{@html displayedHtml}<span class="cursor text-white">_</span></div>
{:else}
<div><span class="text-vscode-purple">const</span> <span class="text-vscode-cyan">aboutMe</span> = {'{'}{'\n'}  <span class="text-vscode-red">name</span>: <span class="text-vscode-green">'Jose Lazo'</span>,{'\n'}  <span class="text-vscode-red">github</span>: <a href="https://github.com/joselazovargas" target="_blank" class="text-vscode-green no-underline">'@joselazovargas'</a>,{'\n'}  <span class="text-vscode-red">linkedin</span>: <a href="https://www.linkedin.com/in/jose-lazo-ict/" target="_blank" class="text-vscode-green no-underline">'@jose-lazo-ict'</a>,{'\n'}  <span class="text-vscode-red">email</span>: <span class="text-vscode-green">'{isVerified ? emailValue : '[hidden]'}'</span>,{'\n'}  <span class="text-vscode-red">readMore</span>: {'{ '} <span class="text-vscode-red">job</span>: <a href="https://black6.com" target="_blank" class="text-vscode-green no-underline">'BLACK6'</a> {' }'}{'\n'}{'};'}{'\n\n'}<span class="text-vscode-purple">const</span> <span class="text-vscode-cyan">statsForNerds</span> = {'{ '}
{#if !statsExpanded}
<button onclick={onExpandStats} class="text-gray-500 cursor-pointer hover:text-white transition-colors italic">... /* click to expand */</button>
{:else}
{'\n'}  <span class="text-vscode-red">coffee</span>: <span class="text-vscode-green">'infinity'</span>,{'\n'}  <span class="text-vscode-red">os</span>: <span class="text-vscode-green">'win32'</span>,{'\n'}  <span class="text-vscode-red">status</span>: <span class="text-vscode-green">'building'</span>{'\n'}
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