# Design System Strategy: The Intellectual Atrium

## 1. Overview & Creative North Star
This design system is an evolution of the "Cognitive Sanctuary" philosophy, tailored specifically for the prestigious, academic atmosphere of the University of Colorado Boulder. Our Creative North Star is **"The Intellectual Atrium."**

Like a modern university library, the UI should feel expansive, quiet, and authoritative. We are moving away from the "template" look of standard web grids. Instead, we embrace **Intentional Asymmetry** and **Editorial Spacing**. We treat the screen as a canvas where high-contrast typography meets layered tonal depth. Elements should feel like they are "resting" on one another rather than being boxed in.

## 2. Color & Atmospheric Depth
The palette is rooted in CU Gold (`primary_container`: #CFB87C) and deep Obsidian Black (`on_primary_fixed`: #241a00). 

### The "No-Line" Rule
To maintain a premium, custom feel, **1px solid borders are strictly prohibited for sectioning.** We do not "box" content. Boundaries must be defined through:
*   **Background Shifts:** Transitioning from `surface` (#f8faf8) to `surface_container_low` (#f3f4f3).
*   **Rhythmic Spacing:** Using the `24` (6rem) or `16` (4rem) spacing tokens to create mental "rooms" for different content types.

### Surface Hierarchy & Nesting
Think of the UI as physical layers of fine stationery. 
*   **Base:** `surface` (#f8faf8) is your canvas.
*   **Secondary Content:** `surface_container` (#edeeed) defines recessed areas.
*   **Interactive Layers:** Use `surface_container_lowest` (#ffffff) for cards to create a soft "lift" against a slightly darker background.

### The "Glass & Gradient" Rule
To add "soul" to the academic scheme:
*   **CTAs:** Use a subtle linear gradient from `primary` (#6e5d2a) to `primary_container` (#cfb87c) at a 45-degree angle. This prevents the gold from looking flat or "muddy."
*   **Overlays:** Use Glassmorphism for floating navigation or modals. Apply `surface_container_lowest` at 80% opacity with a `20px` backdrop-blur. This allows the sophisticated gold and grey tones to bleed through the interface.

## 3. Typography: The Editorial Voice
We use **Manrope** to bridge the gap between academic tradition and modern clarity.

*   **Display Scale (`display-lg` to `display-sm`):** These are your "statements." Use them with `tight` letter spacing (-0.02em) and `on_background` (#191c1c) for an authoritative, journalistic feel.
*   **Headline Scale:** Use these for section starters. Always pair a `headline-lg` with a significant top margin (`spacing-12`) to allow the thought to breathe.
*   **Body Text:** All long-form content must use `body-lg` or `body-md` in `on_surface_variant` (#4c463a) to reduce eye strain, adhering to the "Cognitive Sanctuary" theme.
*   **Labels:** Use `label-md` in all-caps with `0.05em` letter spacing for a "curated gallery" aesthetic.

## 4. Elevation & Depth: Tonal Layering
Traditional shadows are often too "digital." We achieve depth through **Layering Principles**:

*   **Ambient Shadows:** If a floating element (like a FAB or Menu) requires a shadow, use a large blur (32px+) at 4%–8% opacity. The shadow color should not be grey; use a tint of `on_surface` to simulate natural light in a room.
*   **The "Ghost Border" Fallback:** If a border is required for accessibility in input fields, use `outline_variant` (#cec6b6) at **20% opacity**. It should be a whisper, not a shout.
*   **Stacking:** A `surface_container_lowest` card sitting on a `surface_container_low` background is our standard for "elevated" content. No shadow is necessary; the tonal shift is enough.

## 5. Component Guidelines

### Buttons
*   **Primary:** `primary` (#6e5d2a) background with `on_primary` (#ffffff) text. Use `rounded-lg` (0.5rem).
*   **Secondary:** `secondary_container` (#e2e2e2) with `on_secondary_container` (#646464).
*   **Tertiary:** No background. Use `primary` text with a subtle underline or arrow icon.

### Input Fields
*   **Style:** No outer border. Use `surface_container_low` as a solid background fill. 
*   **State:** On focus, transition the background to `surface_container_high` and add a 2px bottom-bar in `primary_container` (CU Gold).

### Cards & Lists
*   **Strict Rule:** No divider lines between list items. Use `spacing-4` (1rem) of vertical whitespace to separate items.
*   **Interaction:** On hover, a card should shift from `surface` to `surface_container_lowest`.

### Chips
*   Use `rounded-full` (9999px). For unselected states, use `surface_container_highest` with `on_surface_variant` text. For selected states, use CU Gold (`primary_container`).

## 6. Do’s and Don'ts

### Do:
*   **Do** use asymmetrical layouts. If an image is on the left, let the text on the right have a wider margin to create a "custom-built" feel.
*   **Do** use the `primary_container` (#cfb87c) as a highlight for progress bars, active states, and icons.
*   **Do** prioritize vertical rhythm. Ensure the space between a title and its body is half the space between that body and the next section.

### Don’t:
*   **Don’t** use pure #000000 for body text; it is too harsh. Use `on_surface` (#191c1c).
*   **Don’t** use 100% opaque borders. They break the "Cognitive Sanctuary" flow.
*   **Don’t** crowd the edges. If a container is near the screen edge, give it at least `spacing-6` (1.5rem) of padding.

---
*Director's Note: This design system is not a set of constraints, but a foundation for excellence. Treat every screen like a page in a high-end academic journal. If it feels "standard," add more whitespace and remove a line.*