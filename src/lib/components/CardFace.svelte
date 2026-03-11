<script lang="ts">
	import Syntax from './Syntax.svelte';
	import CodeProperty from './CodeProperty.svelte';
	import { dev } from '$app/environment';

	interface Props {
		stage: number;
		displayedHtml: string;
		isVerified: boolean;
		statsExpanded: boolean;
		contactInfo: {
			name: string;
			github: string;
			githubUrl: string;
			linkedin: string;
			linkedinUrl: string;
			email: string;
			phone: string;
			job: string;
			jobUrl: string;
		};
		renderTime: number;
		onExpandStats: () => void;
	}

	let { 
		stage, 
		displayedHtml, 
		isVerified, 
		statsExpanded, 
		contactInfo,
		renderTime, 
		onExpandStats
	}: Props = $props();
</script>

<div class="card-content w-full h-full min-h-[150px] flex flex-col justify-center">
{#if stage === 0}
	<div class="flex items-center justify-center py-8">
		<Syntax type="gray">Press any key to continue...</Syntax>
		<span class="cursor text-gray-400 ml-2">_</span>
	</div>
{:else if stage === 1}
	<div class="whitespace-pre-wrap py-2">{@html displayedHtml}<span class="cursor text-white">_</span></div>
{:else}
	<div class="flex flex-col py-2">
		<div class="flex items-center gap-2">
			<Syntax type="purple">const</Syntax> <Syntax type="cyan">aboutMe</Syntax> = {'{' }
		</div>
		
		<CodeProperty name="name" value={contactInfo.name} />
		<CodeProperty name="github" value={contactInfo.github} isLink url={contactInfo.githubUrl} />
		<CodeProperty name="linkedin" value={contactInfo.linkedin} isLink url={contactInfo.linkedinUrl} />
		<CodeProperty name="email" value={contactInfo.email} canCopy type="email" />
		<CodeProperty name="phone" value={contactInfo.phone} canCopy type="phone" />
		
		<div class="flex flex-col pl-[1rem]">
			<div><Syntax type="red">remixes</Syntax>: [</div>
			<div class="pl-[1rem]"><a href="/remixes/webgl-techno" class="text-vscode-green no-underline hover:underline hover:bg-white/5 rounded px-1 transition-colors">'WebGL shader unreal effects'</a>,</div>
			<div class="pl-[1rem]"><a href="/remixes/neon-grid" class="text-vscode-green no-underline hover:underline hover:bg-white/5 rounded px-1 transition-colors">'Neon Grid Matrix'</a>,</div>
			<div class="pl-[1rem]"><a href="/remixes/tui-pixel" class="text-vscode-green no-underline hover:underline hover:bg-white/5 rounded px-1 transition-colors">'pixel style TUI animations'</a>,</div>
			<div class="pl-[1rem]"><a href="/remixes/qr-generator" class="text-vscode-green no-underline hover:underline hover:bg-white/5 rounded px-1 transition-colors">'QR generator'</a></div>
			<div>],</div>
		</div>
		
		{#if dev}
			<CodeProperty name="myWork" value="[]" />
		{/if}
		
		<div class="flex items-center gap-2 pl-[1rem]">
			<Syntax type="red">readMore</Syntax>: {'{ '} 
			<CodeProperty name="job" value={contactInfo.job} isLink url={contactInfo.jobUrl} indent={0} isLast />
			{' }'}
		</div>
		
		<div>{'};'}</div>
		<div class="h-4"></div>
		
		<div class="flex items-center gap-2">
			<Syntax type="purple">const</Syntax> <Syntax type="cyan">statsForNerds</Syntax> = {'{ '}
			{#if !statsExpanded}
				<button onclick={onExpandStats} class="text-gray-500 cursor-pointer hover:text-white transition-colors italic">/* ... */</button>
				{' };'}
			{/if}
		</div>
		
		{#if statsExpanded}
			<CodeProperty name="renderTime" value="{renderTime}ms" />
			<CodeProperty name="captcha" value="{isVerified ? 'passed' : 'waiting...'}" isLast />
			<div>{'};'}</div>
		{/if}
		
		<div class="mt-2">
			<span class="cursor text-white">_</span>
		</div>
	</div>
{/if}
</div>

<style>
	@keyframes blink {
		0%, 50% { opacity: 1; }
		50.01%, 100% { opacity: 0; }
	}
	.cursor {
		display: inline-block;
		animation: blink 1s step-start infinite;
	}
</style>