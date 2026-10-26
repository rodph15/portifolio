# Copilot instructions

## Project overview

- This repository is a static, bilingual portfolio and blog. It has no application build system or package manager.
- GitHub Pages deploys the entire repository from `master` through `.github/workflows/static.yml`.
- Keep changes small, focused, accessible, and compatible with the existing static-site architecture. Do not introduce frameworks, build tooling, or dependencies unless explicitly requested.

## Architecture

- `index.html` is the portfolio home page and includes the initial, no-JavaScript blog-card fallback.
- `blog/` contains one standalone HTML page per full article.
- `content/posts.json` is the canonical blog index; `content/profile.json` contains reusable profile data.
- `assets/js/site.js` owns home-page content, translations, language switching, post loading, sorting, and pagination.
- `assets/js/article.js` owns translations and language switching for all article pages.
- `assets/css/main.css` is the runtime stylesheet. Its Sass source is organized under `assets/sass/`, with `assets/sass/main.scss` as the entry point.
- `assets/js/main.js` and the vendored JavaScript/CSS files support the existing site behavior. Avoid modifying vendored/minified files unless the task specifically requires it.

## Visual and responsive design

- Preserve the dark terminal/Matrix-inspired visual identity, restrained green accents, and existing blinking terminal cursors.
- Do not add a simulated terminal-window rectangle or macOS window-control dots to the header.
- Keep the profile image/name aligned with navigation and retain the `rodolfo@portfolio` title and cursor.
- Keep the home page and every article usable on narrow screens. Check the initial HTML and responsive Sass/CSS so the brand, terminal title, navigation, and language control do not overlap.
- When changing styling, update both `assets/css/main.css` and the corresponding Sass source under `assets/sass/`.

## Language and accessibility

- The site supports English (`en`) and Portuguese (`pt`). Preserve the user's explicit language choice in the shared `portfolio-language` local-storage key.
- Use browser-language detection only when there is no valid saved language choice.
- Any added translatable HTML must use a `data-i18n` key that exists in both language dictionaries. Update `site.js` for the home page and `article.js` for article pages.
- When a translated value contains HTML, preserve the established `innerHTML` behavior and equivalent markup in both languages.
- Keep meaningful labels, keyboard-accessible controls, correct heading structure, alt text, and existing skip-link/navigation accessibility patterns.

## Blog content and pagination

- Full articles belong in `blog/`; the home page must link to their local pages, never to LinkedIn posts.
- Add each article to `content/posts.json` with:
  - a dated filename-matching `slug`;
  - `url` exactly equal to `blog/<slug>.html`;
  - `title`, `summary`, and `category` in both `en` and `pt`;
  - an ISO-8601 `publishedAt` timestamp with an explicit timezone offset.
- Sort posts by parsed `publishedAt`, newest first. Show exactly three posts per page unless explicitly instructed otherwise.
- The first page must show the three newest posts. Adding a post must automatically move older posts to later pages.
- Keep the static fallback in `index.html` limited to the same three newest posts in newest-first order. Update its translations in `site.js` when adding a fallback card.
- Preserve accessible generated pagination: previous/next buttons, numbered buttons, current-page indication, and ellipses for large result sets.
- Bind pagination click handlers directly to generated buttons. Do not rely on delegated `event.target.closest()`, dynamic selectors for the active page, optional chaining, `replaceChildren`, or variadic `append` in this code path; retain broad browser and Safari compatibility.
- Article pages must follow the existing structure: localized category, date/time, title, summary, tags, full body, closing prompt, and return links. Use article-specific translation keys rather than keys for another article.
- For each article, provide consistent Open Graph, Twitter, author, and `BlogPosting` JSON-LD metadata. Do not invent a canonical URL or `og:url` when the production URL is unknown.

## Content and external publishing

- Treat the portfolio as the source for full articles; LinkedIn promotions are concise, distinct summaries that point to a verified public article URL.
- For LinkedIn article promotions, create an original reviewed 1200x627 cover image with the portfolio's dark terminal style, restrained green accents, an English headline, relevant simple graphic, author name, and descriptive alt text.
- Place temporary LinkedIn drafts and images in the ignored `.linkedin-drafts/` directory. Do not publish until the user explicitly approves the exact post text and image.

## Validation

- For changed JavaScript, run `node --check` on each changed file.
- For article or index changes, parse `content/posts.json`; parse every changed JSON-LD block; verify every page `data-i18n` key has `en` and `pt` values; and verify the post index URL matches the article file.
- For blog listing changes or new posts, verify newest-first sorting, exact three-item page slices, previous/next/numbered controls, current-page state, ellipses, and the three-post static fallback order.
- Run `git diff --check` for source changes.
- When testing the site in a browser, serve it over HTTP (for example, `python3 -m http.server`); do not use `file://`, which blocks `fetch()` for `content/posts.json`.
- Cache-bust changed JavaScript or CSS references in `index.html` when needed. Remember that uncommitted local changes are not deployed to GitHub Pages.

## Change hygiene

- Read and preserve existing conventions before editing. Reuse the site's existing HTML, CSS, JavaScript, localization, and metadata patterns instead of duplicating or redesigning them.
- Do not remove or revert unrelated working-tree changes.
- Avoid invented URLs, unverified factual claims, and placeholder production content. Surface uncertainties and ask for the required authoritative information when it cannot be verified.
