# System Architecture: Minimal Typing Card

## 1. Design System: "Pure Focus"
The UI is built on a minimal, high-contrast paradigm centered around a single interactive element.

### Core Visual Utilities
- **VSCode Colors**: Theme variables for syntax highlighting (blue, green, purple, cyan, red) used within the typewriter reveal.

## 2. State Machine: Interactive Lifecycle
The application operates on a 3-stage linear state machine:

| Stage | Label | Logic |
| :--- | :--- | :--- |
| **0** | `WAITING` | Renders the "Press any key to continue" prompt. |
| **1** | `TYPING` | Staggers the rendering of HTML tokens using a `setInterval` loop (100ms interval). |
| **2** | `COMPLETE` | Ends the animation and renders the full interactive profile with live links. |

## 3. Tech Stack
- **Framework**: Svelte 5 (Runes-based state management).
- **Styling**: Tailwind CSS v4.
- **Interactions**: Vanilla JS event listeners and timers.

## 4. Components
- `InteractiveCard`: The main engine combining the 3D transforms, lighting effect, and typing logic.
