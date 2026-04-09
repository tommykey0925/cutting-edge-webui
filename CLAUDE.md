# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

NEXUS is a static website showcasing a futuristic UI design. It is built with plain HTML, CSS, and vanilla JavaScript — no build tools, frameworks, or package managers.

## Development

Open `index.html` directly in a browser to preview. No build step or dev server is required.

## Architecture

- **Pages**: `index.html` (landing/hero page), `products.html` (pricing cards + comparison table)
- **Styles**: `style.css` (shared global styles — layout, glassmorphism, animations, responsive breakpoints), `products.css` (products page-specific styles)
- **JavaScript**: Inline `<script>` tags at the bottom of each HTML file (cursor glow effect, parallax on index page)
- **Navigation**: Shared nav structure duplicated in each HTML file (no templating)

## Design System

- Dark theme with CSS custom properties defined in `:root` (e.g. `--bg-dark`, `--accent-cyan`, `--gradient-main`)
- Glassmorphism pattern: `backdrop-filter: blur()` + semi-transparent `rgba()` backgrounds + thin borders
- Gradient text via `background-clip: text` + `color: transparent`
- Language: Japanese for body text, English for headings and brand names

## Key CSS Patterns

- CSS Grid with `auto-fit` + `minmax()` for responsive card layouts
- `@keyframes` animations for floating orbs, hologram rings, sphere pulse, shimmer
- Responsive breakpoints at 1024px and 768px
- Custom scrollbar styling via `::-webkit-scrollbar`
