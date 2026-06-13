# JSCC 2026 - Awesome DX Session
> Small session where we discussed tools or other awesome developer experiences we had the past year (some derailing included)
- **Bun**
    - *AI Description: All-in-one JS/TS runtime + toolkit, drop-in Node replacement, written in Zig for speed.*
    - What does it do?
        - makes everything faster
        - package manager
        - Test Runner
        - Own SQL lib

- Problem: SSR in next is complicated; static builds; interactive applications
    - **qwik**
        - *AI Description: Resumable framework — ships near-zero JS, "resumes" server state on client instead of hydrating.*
    - **Angular w/ Hydration**
        - *AI Description: Angular's non-destructive hydration reuses server-rendered DOM instead of re-rendering on load.*
    - **Hand written JS, HTML, CSS**
        - Maybe w/ webcomponents
        - *AI Description: Web Components = native browser API for reusable custom elements, no framework needed.*
    - **Vike**
        - *AI Description: Framework-agnostic SSR/SSG meta-framework (formerly vite-plugin-ssr), Vite-based.*
    - **htmx**
        - *AI Description: Adds AJAX/SSE/swaps via HTML attributes — interactivity without writing JS.*
    - **Astro**
        - Metaframework
        - Static Site Generator + Islands of Interactivity
        - *AI Description: Content-first framework; ships HTML by default, hydrates only interactive "islands". Multi-framework (React/Vue/Svelte) support.*

- **git rerere**
    - *AI Description: "Reuse recorded resolution" — git remembers how you fixed a merge conflict, auto-applies it next time.*
- **mob.sh**
    - *AI Description: CLI for mob/pair programming — smooth git handoff between drivers via one shared branch.*
- **zoxide**
    - *AI Description: Smarter `cd` — jumps to frequent/recent dirs by partial name (`z proj`). Rust.*
- **Amethyst**
    - *AI Description: Tiling window manager for macOS, automatic layouts (xmonad-inspired). (note: spelled "Amethyst")*
- **starship.rs**
    - *AI Description: Fast cross-shell prompt, customizable, shows git/lang/context. Rust. (note: spelled "starship")*
- **zellij** (tmux alternative)
    - *AI Description: Terminal multiplexer + workspace, batteries-included with discoverable keybinds and layouts. Rust.*
    - **cmux** (https://cmux.com/de)
        - *AI Description: Terminal/session tool — couldn't verify from notes; confirm before relying on description.*

# ideas (not discussed)
- peon-ping
