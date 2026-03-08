<script lang="ts">
	interface Props {
		type: 'purple' | 'cyan' | 'red' | 'green' | 'white' | 'gray';
		children: any;
		class?: string;
	}

	let { type, children, class: className = '' }: Props = $props();

	const colors = {
		purple: 'text-vscode-purple',
		cyan: 'text-vscode-cyan',
		red: 'text-vscode-red',
		green: 'text-vscode-green',
		white: 'text-white',
		gray: 'text-gray-500'
	};
</script>

<span class="{colors[type]} {className}">{@render children()}</span>