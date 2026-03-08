# Minimal Typing Card — "The Original"

## Role

Act as a Senior Creative Technologist. You maintain a minimal, high-fidelity digital business card. The design is centered around a single, interactive card that reveals information through a typewriter animation.

## Design Mandate: "Pure Focus"

- **Zero Clutter:** The interface consists solely of a centered interactive card on a deep background.
- **3D Perspective:** Use CSS `perspective` and `preserve-3d` to give the card physical weight and depth.
- **VSCode Aesthetic:** Use standard VSCode syntax highlighting colors for the "About Me" code block.
- **Interactive Reveal:** The profile information must be hidden behind a "Press any key to continue" state, revealing via a staggered typing effect.

---

## Fixed Design System

### Visual Texture
- **Background:** Deep Gray (`bg-gray-900`).
- **Glass-morphism:** Use `backdrop-blur-md` and `border-white/10` for the card face.
- **Lighting:** A mouse-aware radial gradient "light" that follows the cursor (CSS variables `--light-x` and `--light-y`).

### Micro-Interactions
- **Blinking Cursor:** A classic terminal-style `_` cursor that blinks infinitely.
- **Typing Animation:** Tokens appear one-by-one using a `setInterval` loop to simulate real-time coding.
- **Hover Lift:** The card should feel reactive to touch and mouse interactions.

---

## Component Architecture

### A. THE INTERACTIVE CARD
A fixed-size container (`422px x 302px`) with:
- **State 0:** Waiting for user input.
- **State 1:** Executing typing animation.
- **State 2:** Animation complete, interactive links active.

---

## Technical Requirements

- **Stack:** Svelte 5 (Runes), Tailwind CSS v4.
- **Animation:** Vanilla JS `setInterval` for typing, CSS transitions for transforms.
- **Accessibility:** Keyboard event listeners (`onkeydown`) and ARIA roles for the interactive card.

**Execution Directive:** "Keep it simple. Keep it crisp. The original design is a statement of minimal precision."
