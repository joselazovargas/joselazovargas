<script lang="ts">
	import { onMount } from 'svelte';
	import Syntax from '$lib/components/Syntax.svelte';

	let canvas: HTMLCanvasElement;
	let gl: WebGLRenderingContext | null;

	const vertexShaderSource = `
		attribute vec2 position;
		void main() {
			gl_Position = vec4(position, 0.0, 1.0);
		}
	`;

	const fragmentShaderSource = `
		precision highp float;
		uniform float time;
		uniform vec2 resolution;

		void main() {
			vec2 uv = (gl_FragCoord.xy * 2.0 - resolution.xy) / min(resolution.x, resolution.y);
			
			// Grid effect
			vec2 gv = fract(uv * 10.0 + time * 0.2) - 0.5;
			float grid = 0.01 / abs(gv.x * gv.y);
			
			// Glowing lines
			float line = smoothstep(0.1, 0.0, abs(uv.x + sin(uv.y + time) * 0.5));
			
			vec3 color = vec3(0.0, 0.5, 1.0) * grid * 0.2; // Blue grid
			color += vec3(1.0, 0.0, 0.5) * line; // Pink line
			
			// Distance fade
			color *= exp(-length(uv) * 0.5);
			
			gl_FragColor = vec4(color, 1.0);
		}
	`;

	function createShader(gl: WebGLRenderingContext, type: number, source: string) {
		const shader = gl.createShader(type)!;
		gl.shaderSource(shader, source);
		gl.compileShader(shader);
		return shader;
	}

	onMount(() => {
		gl = canvas.getContext('webgl');
		if (!gl) return;

		const program = gl.createProgram()!;
		gl.attachShader(program, createShader(gl, gl.VERTEX_SHADER, vertexShaderSource));
		gl.attachShader(program, createShader(gl, gl.FRAGMENT_SHADER, fragmentShaderSource));
		gl.linkProgram(program);
		gl.useProgram(program);

		const positionBuffer = gl.createBuffer();
		gl.bindBuffer(gl.ARRAY_BUFFER, positionBuffer);
		gl.bufferData(gl.ARRAY_BUFFER, new Float32Array([-1, -1, 1, -1, -1, 1, -1, 1, 1, -1, 1, 1]), gl.STATIC_DRAW);

		const positionLocation = gl.getAttribLocation(program, 'position');
		gl.enableVertexAttribArray(positionLocation);
		gl.vertexAttribPointer(positionLocation, 2, gl.FLOAT, false, 0, 0);

		const timeLocation = gl.getUniformLocation(program, 'time');
		const resolutionLocation = gl.getUniformLocation(program, 'resolution');

		function render(now: number) {
			if (!gl) return;
			gl.uniform1f(timeLocation, now * 0.001);
			gl.uniform2f(resolutionLocation, canvas.width, canvas.height);
			gl.drawArrays(gl.TRIANGLES, 0, 6);
			requestAnimationFrame(render);
		}
		requestAnimationFrame(render);

		const resize = () => {
			if (!canvas) return;
			canvas.width = canvas.clientWidth;
			canvas.height = canvas.clientHeight;
			gl?.viewport(0, 0, canvas.width, canvas.height);
		};
		window.addEventListener('resize', resize);
		resize();

		return () => window.removeEventListener('resize', resize);
	});
</script>

<div class="min-h-screen bg-black text-white font-mono p-4 md:p-12 flex flex-col items-center overflow-x-hidden">
	<div class="w-full max-w-6xl z-10">
		<a href="/" class="text-vscode-purple hover:underline mb-8 inline-block">← cd /home</a>
		
		<div class="mb-4 flex items-center gap-2 text-lg">
			<Syntax type="purple">export</Syntax> 
			<Syntax type="purple">default</Syntax> 
			<Syntax type="purple">const</Syntax> 
			<span style="view-transition-name: remix-neon-grid;">
				<Syntax type="cyan">NeonGridMatrix</Syntax>
			</span> = () => {' {'}
		</div>

		<div class="pl-4 md:pl-8 border-l border-white/10 ml-2 space-y-8 relative min-h-[600px]" style="view-transition-name: experiment-body;">
			<div class="absolute inset-0 z-0 opacity-80 rounded-xl overflow-hidden border border-white/10 shadow-2xl">
				<canvas bind:this={canvas} class="w-full h-full"></canvas>
			</div>

			<div class="relative z-10 p-8 pointer-events-none">
				<div class="backdrop-blur-md bg-black/40 border border-white/10 p-6 rounded-xl max-w-sm">
					<Syntax type="gray">/* Retro-futuristic neon grid simulation. */</Syntax>
					<div class="mt-4 space-y-2 text-xs">
						<div class="flex justify-between">
							<span class="text-vscode-red">grid_density</span>
							<span class="text-vscode-cyan">10.0</span>
						</div>
						<div class="flex justify-between">
							<span class="text-vscode-red">neon_flux</span>
							<span class="text-vscode-cyan">active</span>
						</div>
					</div>
				</div>
			</div>
		</div>

		<div class="mt-4 text-lg">
			{'}'}
		</div>
	</div>
</div>

<style>
	canvas {
		display: block;
	}
</style>