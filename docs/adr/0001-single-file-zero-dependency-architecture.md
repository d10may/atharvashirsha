# Single-file zero-dependency architecture

The application is delivered as a single self-contained HTML file containing all markup, styles, scripts, and hymn corpus data, relying only on web fonts for typography. We decided against bundlers, frameworks (React/Vue), and external package managers in order to enable instant portability across devices, offline local file execution (`file://`), and zero-setup deployment via GitHub Pages.

## Considered Options

- **Modern SPA framework (React / Svelte + Vite)**: Rejected because it introduces build toolchains, node_modules dependencies, and deployment overhead for a lightweight text recitation companion.
- **Multi-file static distribution (split HTML/CSS/JS)**: Rejected for distribution simplicity; a single file can be downloaded or emailed directly and opened on any smartphone, tablet, or TV browser without a local web server.

## Consequences

- The entire application lives within `index.html`, making it trivial to host, share, or run offline.
- Componentization is maintained via clean functional scoping and CSS custom properties rather than framework abstractions.
