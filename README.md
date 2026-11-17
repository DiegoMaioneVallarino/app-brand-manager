# app-brand-manager v0.1.0

A dependency-free Node.js CLI for scanning CSS styles and applying explicit brand token mappings. Requires Node.js 20+.

## Installation

```bash
cd app-brand-manager
npm link
```

Or without linking:

```bash
node src/cli.js scan ./examples
```

## Commands

```bash
app-brand-manager scan ./examples --out scan.json
app-brand-manager preview ./examples --brand ./brands/paix.brand.json
app-brand-manager apply ./examples --brand ./brands/paix.brand.json
```

**IMPORTANT:** `apply` writes modified CSS and `.app-brand-manager/brand.css`. The CLI does not automatically import the generated stylesheet. Add an import to the app's main CSS file (adjust relative path to suit its location):

```css
@import "../.app-brand-manager/brand.css";
```

Or import from JS/TS using the correct relative path. Make sure the generated CSS is included in the production bundle.

## How mappings work

Brand tokens contain target values. `mappings` explicitly associate an original value with a token name. For example, `"#8b5cf6": "primary"` in `mappings.colors` converts matching color declarations into `var(--abm-colors-primary)`. Values without mappings remain unchanged. Edit the included example brand file to match your actual Paix styles; its values are illustrative, not extracted from the Paix project.

## Safety and limitations

- Only `.css` files are scanned. No `.scss`, `.less`, `.tsx`, `.jsx`, inline styles, Tailwind classes or CSS-in-JS yet.
- Mappings are value-based and intentionally conservative. This can conflate semantically different uses of the same color; inspect the preview report.
- Colors are replaced inside supported color-related declarations; other categories require an exact declaration-value match.
- Does not attempt layout changes or semantic component classification.
- CSS parsing is a lightweight scanner, not a complete CSS parser. For unusual syntax, inspect the resulting diff.
- Original files are copied into `.app-brand-manager/backups/<timestamp>/` before modifications. Existing files are not deleted.
- Commit your project before running `apply`. Use `preview` first.
- Do not run `apply` repeatedly on files already transformed unless you understand the mappings; the original token references are not mapped a second time.

## Tests

```bash
npm test
```
"# app-brand-manager" 
