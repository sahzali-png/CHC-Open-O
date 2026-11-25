# CHC brand rules for the CHC front end

Source of truth: Canadian Heart Care Design System — https://claude.ai/artifact/GA8VvDmv1jY95XBeXhKpqM
Tokens copied here: `tokens.json` (every value) and `tokens.css` (`--red-500`, `--text-primary`, …).
Use the CSS custom properties; never paste raw hex values into screens.

## Colour
- Heart Red `--red-500` #E1313E (brand, active nav, primary emphasis); Deep Red `--red-600` #C81D2B.
- Caring-Hands Blue `--blue-500` #1C82C6 (secondary; viewer/imaging actions, info).
- Vital Orange `--orange-500` #F37C20 — the main call-to-action colour (New encounter, Confirm & book).
  Text on orange buttons is ink black (#0A0C0E) for contrast.
- Ink `--ink-950` #0A0C0E for the sidebar and dark surfaces; cool charcoal neutrals for text/borders.
- Status colours always come with a word (never colour alone).

## Tagline
"Your Heart in Caring Hands" is **always orange text, or black text on an orange bar.** Never other colours.

## Type
- Display: Bunken Tech Sans Pro (licensed — confirm web licence before shipping), UPPERCASE ITALIC for
  page titles, eyebrows and primary buttons. Eyebrows use a `//` prefix, e.g. `// TODAY'S SCHEDULE`.
- Body/UI: Jost (substitute for Century Gothic). Labels: uppercase, letter-spacing 0.06em.

## Shape
- Cards: white, 1 px border `--ink-100`, radius 18 px, soft shadow.
- Inputs: radius 8 px. Buttons, chips, search: full pill (999 px).
- Signature gradient (`--grad-flyer`, red → deep red → black) only for hero bands, e.g. dialog headers.

## Accessibility
Real buttons/links/inputs with labels; text contrast ≥ 4.5:1; touch targets ≥ 44 px;
keyboard usable; no emoji.

## Logo
Use the official logo files from the design system (logo mark in a white circle on dark).
