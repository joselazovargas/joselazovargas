<script lang="ts">
    import { onMount } from "svelte";

    let panel: HTMLDivElement;
    let stage = 0; // 0 = waiting, 1 = typing, 2 = done
    let displayedHtml = "";
    let cursorVisible = true;

    // Syntax‐colored tokens for your contact block
    const tokens = [
        `<span class="text-vscode-purple">const</span>`,
        ` `,
        `<span class="text-vscode-cyan">aboutMe</span>`,
        ` `,
        `= `,
        `{`,
        `\n  `,
        `<span class="text-vscode-red">name</span>`,
        `: `,
        `<span class="text-vscode-green">'Jose Lazo'</span>`,
        `,`,
        `\n  `,
        `<span class="text-vscode-red">job</span>`,
        `: `,
        `<a href="https://black6.com" target="_blank" class="text-vscode-green no-underline">'BLACK6'</a>`,
        `,`,
        `\n  `,
        `<span class="text-vscode-red">github</span>`,
        `: `,
        `<a href="https://github.com/joselazovargas" target="_blank" class="text-vscode-green no-underline">'@joselazovargas'</a>`,
        `,`,
        `\n  `,
        `<span class="text-vscode-red">linkedin</span>`,
        `: `,
        `<a href="https://www.linkedin.com/in/jose-lazo-ict/" target="_blank" class="text-vscode-green no-underline">'@jose-lazo-ict'</a>`,
        `\n`,
        `}`,
    ];

    // Start typewriter on first safe keypress
    function handleKeydown(e: KeyboardEvent) {
        if (stage !== 0) return;
        if (["Shift", "Control", "Alt", "Meta"].includes(e.key)) return;
        stage = 1;
        let i = 0;
        const interval = setInterval(() => {
            displayedHtml += tokens[i++];
            if (i === tokens.length) {
                clearInterval(interval);
                stage = 2;
                setInterval(() => (cursorVisible = !cursorVisible), 500);
            }
        }, 100);
    }

    onMount(() => {
        if (!panel) return;
        window.addEventListener("keydown", handleKeydown);
        return () => {
            window.removeEventListener("keydown", handleKeydown);
        };
    });
</script>

<div class="flex items-center justify-center w-full h-[100svh] bg-gray-900 p-5">
    <!-- mat + perspective container -->
    <div
        class="relative w-[422px] h-[302px] overflow-hidden rounded-lg"
        style="perspective:600px;"
    >
        <!-- 3D‐tilting card -->
        <div
            bind:this={panel}
            class="card absolute inset-0 cursor-default
             transition-transform duration-500 ease-out will-change-transform"
            style="transform-style: preserve-3d;
             --light-x:50%; --light-y:50%;"
        >
            <!-- moving spotlight -->
            <div class="light"></div>

            <!-- single “face” with terminal/typewriter -->
            <div
                class="face absolute inset-0
                backdrop-blur-md
               rounded-md shadow-xl p-4 font-mono text-white
               overflow-auto whitespace-pre-wrap "
            >
                {#if stage === 0}
                    <div class="flex h-full">
                        <span class="text-gray-400 text-sm md:text-lg"
                            >Press any key to continue...</span
                        >
                        <span class="cursor text-gray-400 text-lg ml-2">_</span>
                    </div>
                {:else}
                    <div>
                        {@html displayedHtml}{stage < 2 && cursorVisible
                            ? "_"
                            : ""}
                    </div>
                {/if}
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
    /* .card:hover .light {
        opacity: 0.8;
    } */
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
