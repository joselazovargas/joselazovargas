<script lang="ts">
    import { onMount } from "svelte";

    let panel: HTMLDivElement;
    let flipped = false;
    let hovering = false;

    function updateTransform(xDeg = 0, yDeg = 0) {
        const flipAngle = flipped ? 180 : 0;
        const scale = hovering ? 1.3 : 1;
        panel.style.transform =
            `scale(${scale}) ` +
            `rotateX(${xDeg}deg) ` +
            `rotateY(${yDeg + flipAngle}deg)`;
    }

    function handleClick() {
        flipped = !flipped;
        updateTransform(0, 0);
    }

    function handleKeydown(e: KeyboardEvent) {
        // ignore pure modifiers
        if (["Shift", "Control", "Alt", "Meta"].includes(e.key)) return;
        flipped = !flipped;
        updateTransform(0, 0);
    }

    onMount(() => {
        if (!panel) return;

        panel.addEventListener("click", handleClick);
        window.addEventListener("keydown", handleKeydown);
        return () => {
            panel.removeEventListener("click", handleClick);
            window.removeEventListener("keydown", handleKeydown);
        };
    });
</script>

<div class="flex items-center justify-center w-full h-[100svh] bg-gray-900 p-5">
    <!-- mat + perspective container -->
    <div
        class="relative w-[422px] h-[302px] overflow-hidden rounded-lg bg-gray-800"
        style="perspective:600px;"
    >
        <!-- rotating/flipping card -->
        <div
            bind:this={panel}
            class="card absolute inset-0 cursor-pointer
             transition-transform duration-500 ease-out will-change-transform"
            style="transform-style: preserve-3d;
             --light-x:50%;
             --light-y:50%;"
        >
            <!-- spotlight -->
            <div class="light"></div>

            <!-- FRONT FACE: terminal stub -->
            <div
                class="face front absolute inset-0
               bg-black/20 backdrop-blur-md border border-white/10
               rounded-md shadow-xl p-4"
            >
                <div class="flex h-full items-center justify-center">
                    <span class="text-gray-400 text-sm font-mono mt-2">
                        Press any key to continue...
                    </span>
                    <span class="cursor text-gray-400 text-xl font-mono">_</span>
                </div>
            </div>

            <!-- BACK FACE -->
            <div
                class="face back absolute inset-0
               bg-black/20 backdrop-blur-md border border-white/10
               rounded-md shadow-xl p-4 font-mono text-white flex items-center justify-center"
                style="transform:rotateY(180deg);"
            >
                <div class="">
                    <p>
                        <span class="text-vscode-purple">const</span> aboutMe
                        <span class="text-vscode-cyan"> = </span>{"{"}
                    </p>
                    <p class="pl-3">
                        <span class="text-vscode-red">name</span><span
                            class="text-vscode-cyan">:</span
                        >
                        <span class="text-vscode-green">'Jose Lazo'</span>,
                    </p>
                    <p class="pl-3">
                        <span class="text-vscode-red">github</span><span
                            class="text-vscode-cyan">:</span
                        >
                        <a href="https://github.com/joselazovargas" target="_blank"><span class="text-vscode-green">'https://github.com/joselazovargas'</span></a>,
                    </p>
                    <!-- <p class="pl-3">
                        <span class="text-vscode-red">email</span><span
                            class="text-vscode-cyan">:</span
                        >
                        <span class="text-vscode-green"
                            >'jose.lazo.vargas@gmail.com'</span
                        >,
                    </p> -->
                    <p>{"}"}</p>
                </div>
            </div>
        </div>
    </div>
</div>

<style>
    .card {
        transform-style: preserve-3d;
        -webkit-transform-style: preserve-3d;
    }
    .face {
        backface-visibility: hidden;
        -webkit-backface-visibility: hidden;
    }
    .light {
        position: absolute;
        width: 200%;
        height: 200%;
        top: -50%;
        left: -50%;
        pointer-events: none;
        background: radial-gradient(
            circle at var(--light-x) var(--light-y),
            rgba(255, 255, 255, 0.15),
            transparent 80%
        );
        opacity: 0;
        transition: opacity 0.3s ease-out;
    }
    .card:hover .light {
        opacity: 0.8;
    }

    /* blinking cursor */
    @keyframes blink {
        0%,
        50% {
            opacity: 1;
        }
        50.01%,
        100% {
            opacity: 0;
        }
    }
    .cursor {
        display: inline-block;
        animation: blink 1s step-start infinite;
    }
</style>
