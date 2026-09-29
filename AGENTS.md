# PixelNaadu project guide

This document is the continuity reference for anyone working on this repository. Read it before changing the site or deploying it.

## Product and messaging

PixelNaadu is a community for next-gen game makers from Kerala. It helps people learn game creation, collaborate, test work and share Kerala's culture, stories and lived experiences through original indie games.

Use the brand name exactly as **PixelNaadu** in visible copy and metadata. Lowercase `pixelnaadu` is correct only in technical identifiers and URLs such as `pixelnaadu.com`, package names and the Instagram handle.

Current key copy:

- Browser title and hero message: `Build Games, Inspire Others`
- Hero description: `PixelNaadu aims to empower next-gen game makers from Kerala to turn our culture, stories and lived experiences into original indie games for the world.`
- Community heading: `Build Together, Grow Together`
- Text buttons use sentence case: `Start learning`, `Join community`
- Instagram and YouTube use icon-only buttons with accessible labels and titles.
- Avoid em dashes in visible copy.

## Technology

The site is a static Jekyll site with a Svelte resource explorer and Bulma styling.

- Jekyll 3.10 renders pages and injects `_data/resources.json` into the page.
- Vite bundles Svelte, SCSS, fonts and the pixel icon font.
- Svelte 5 powers only the interactive learning-resource section.
- Bulma provides base, button and container primitives.
- Pixelify Sans is used for display text.
- Space Grotesk is used for body text.
- Pixelarticons supplies the local pixel icons.

Important source files:

- `index.html`: page structure, hero, community section and footer
- `community.html`: `/community` redirect to Discord
- `_layouts/default.html`: shared HTML document shell
- `_src/App.svelte`: learning categories, selection and resource cards
- `_src/main.js`: Svelte mounting, mobile navigation and lunar-world canvas rendering
- `_src/styles.scss`: all site styles and design tokens
- `_data/resources.json`: curated learning resources
- `vite.config.js`: deterministic output to `assets/dist`
- `_config.yml`: Jekyll settings and exclusions
- `CNAME`: custom-domain file copied into every build

## Design system

The visual direction is warm Kerala-inspired pixel art, not dark blue or a generic technology palette.

Core colors are defined in `_src/styles.scss`:

- Marigold: `#f5ad2f`
- Coir brown: `#321a12`
- Cream: `#fff1cf`
- Paper: `#fff9e9`
- Coral: `#df674d`
- Laterite: `#a8442f`
- Leaf green: `#507a45`
- Dark leaf: `#2f5638`

Current hero details:

- The header and hero share the marigold background.
- `Build Games,` is white.
- `Inspire Others` uses coir brown.
- Both retain an offset pixel shadow. Do not add an outline unless requested.
- The hero background is rendered from the Lunar Surface 1-B Tiled map and tileset.
- The earlier Free 1 Bit Forest experiment remains commented out in `index.html` for reference.

Brand and art assets:

- `assets/images/logo.svg` is the standalone palm-tree mark.
- The visible wordmark is text beside the mark, not part of the SVG.
- `assets/images/LunarSurface1BTileset/` contains the active hero art and map.
- `assets/images/itch-tropical-sunset.png` is used in the community section.
- Art attribution is retained in the footer.
- Check accompanying readme, licence and source files before reusing or replacing third-party art.

## Learning content

The explorer has four categories. The `id` in `_src/App.svelte` must exactly match the `category` value in `_data/resources.json`.

| Display label | Data category |
| --- | --- |
| Design the Play | `Design play` |
| Create Game Art | `Create game art` |
| Build the Game | `Build the game` |
| Share and Sell It | `Sell your game` |

Content principles:

- Include only free resources or resources with meaningful free access.
- Prefer official documentation, respected free courses, active GitHub awesome lists and resources repeatedly recommended in relevant Reddit communities.
- Verify that links and claims are current before adding them.
- Godot and Unity are both supported in the building section.
- Art coverage includes pixel art and Blender-based 3D work.
- Do not add GDevelop resources unless the product direction changes.
- Cards are intentionally simple, equal-height and entirely clickable.
- Do not restore thumbnails, tags, star counts, numbering, metadata grids or `PICK` badges to the rendered cards unless explicitly requested.

## Community and external links

Current destinations:

- Website: `https://pixelnaadu.com`
- Discord: `https://discord.gg/GxfUyJnSr6`
- Instagram: `https://www.instagram.com/pixelnaadu/`
- YouTube: `https://www.youtube.com/@pixelnaadu`
- AIBodh: `https://aibodh.com`

`/community` is implemented by `community.html` and immediately redirects to Discord using JavaScript and an HTML meta refresh, with a normal fallback link.

Instagram links are ordinary external links. Do not add Instagram embeds, remote scripts, pixels or widgets. This keeps the PixelNaadu page free of Instagram cookies until the visitor chooses to leave the site.

External links opened in new tabs should use `rel="noopener noreferrer"` or at minimum `rel="noreferrer"`.

## Build and local development

Install dependencies:

```bash
npm install
bundle install
```

Build everything:

```bash
npm run build
```

Run the required verification after changes:

```bash
npm run check
```

This runs the Vite build and then the Jekyll build. A successful build creates:

- `assets/dist/app.js`
- `assets/dist/app.css`
- bundled fonts under `assets/dist/fonts/`
- the complete deployable site under `_site/`

For local development, use two terminals:

```bash
npm run dev:assets
```

```bash
npm run dev:site
```

Generated directories are intentionally ignored by the private source repository. Do not hand-edit `assets/dist`. Make changes in `_src` and rebuild.

## Repository and deployment arrangement

There are two separate Git repositories in this working tree.

### Private source repository

- Location: project root
- Remote: `git@github.com:jamesfebin/pixelnaaduSource.git`
- Contains the complete Jekyll, Svelte and asset source
- Must remain private
- Ignores `_site/` and `assets/dist/`

### Public build-only repository

- Location: `_site/.git`
- Remote: `git@github.com:jamesfebin/pixelnaadu.git`
- Contains only generated static files
- GitHub Pages publishes the `main` branch from `/ (root)`
- Never copy private source files into this repository

The normal release process is:

```bash
# From the project root
npm run check

# Publish only the generated site
cd _site
git status
git add -A
git commit -m "Deploy site updates"
git push
```

Inspect the `_site` diff before committing. Expected deployment contents include `index.html`, `community/index.html`, `assets/` and `CNAME`.

Do not force-push the public repository. GitHub may create a remote `CNAME` commit when the custom domain is changed. If local and remote histories diverge, reconcile safely with:

```bash
git pull --rebase origin main
git push -u origin main
```

Do not delete the root `CNAME` source file. It contains `pixelnaadu.com` and ensures Jekyll recreates `_site/CNAME` on every build.

## Domain and GitHub Pages

The production domain is `pixelnaadu.com`, registered and managed through Namecheap.

Namecheap DNS records:

| Type | Host | Value |
| --- | --- | --- |
| A | `@` | `185.199.108.153` |
| A | `@` | `185.199.109.153` |
| A | `@` | `185.199.110.153` |
| A | `@` | `185.199.111.153` |
| CNAME | `www` | `jamesfebin.github.io` |

GitHub Pages is configured with `pixelnaadu.com` as the custom domain. The `www` CNAME does not conflict with other Pages repositories. GitHub routes requests by hostname.

DNS and TLS changes can take time to propagate. Do not repeatedly alter correct DNS records while GitHub is provisioning its certificate. Enable `Enforce HTTPS` after certificate issuance is complete.

## Change checklist

Before handing off a change:

1. Preserve the exact `PixelNaadu` brand casing in visible content.
2. Check desktop and mobile behavior for layout changes.
3. Keep navigation and full-card interactions keyboard accessible.
4. Verify external links and use safe new-tab attributes.
5. Run `npm run check`.
6. Confirm `_site/CNAME` still contains `pixelnaadu.com`.
7. Do not push or deploy unless the user asks for it.
