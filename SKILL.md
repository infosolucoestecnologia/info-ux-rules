---
name: info-ux-rules
description: Use when the user asks to review, critique, redesign, or build a screen, dashboard, form, list, table, or component for Info Soluções systems (JS, Info Cardápio, Info Ponto, Sistema Info, Dashboard V2), or says things like 'review this screen', 'does this look right on mobile', 'improve this layout', 'revisa essa tela'. Covers upload, buttons, tables on mobile, modal vs sheet, hierarchy, search and filter, bulk actions, long forms, hover on touch, typography, and required light and dark themes.
---

# info-ux-rules

UX/UI rules for the Info Soluções systems. One file, organized by topic. Apply the core principles to every screen, then read only the sections that match what the screen contains.

## How to use this skill

1. Identify what the screen contains (table, form, list, dashboard, kanban, upload, menu).
2. Read **Core principles**, then only the matching sections below.
3. When reviewing an existing screen, report problems worst first, then fix them. Say which rule each fix comes from.
4. When two rules seem to conflict, the notes marked **Decision** win over the rest of the file.
5. Every screen must work on mobile and on desktop, in light and dark theme.

## Core principles

- **Do not repeat what does not vary.** Information that is identical in every row or card (the same priority "Medium", the same tag "Cold", the same channel icon) must not be visible. Show it only when the value is an exception.
- **One navigation, one search.** Never duplicate the same destinations in the sidebar, top tabs and bottom bar. Never put a search icon in the header plus a search field below it.
- **Nothing essential lives only in hover.** Actions, information and state must be reachable by touch and by keyboard.
- **Color has a function.** Green positive/paid, red negative/overdue, amber pending. Strong color is for what still needs action; resolved items are neutral.
- **Detect capability, not screen width,** for touch and hover (`pointer`, `hover` media queries).
- **If everything has an alert, nothing has one.** A red dot without a number on several menu items at once means nothing.

## Themes and typography

- Every screen supports light and dark theme. Do not ship dark only.
- Implement colors (background, surface, text) as CSS custom properties and swap the value set per theme. Never change the layout structure between themes.
- Typeface for Dashboard V2: Manrope (chosen after a side-by-side test against the system font and Inter).

## Visual direction: Dashboard V2 (owner dashboard)

Goal for the dark theme: premium, sophisticated, not "newspaper-like", not neon. Light is the default theme; this section describes the dark variant.

- Base: near-black neutral background without a strong blue tint (for example `#0A0A0B` or `#0D0D10`), not saturated navy.
- Surfaces: 2 to 3 tones above the background (for example `#0A0A0B` background, `#131316` card, `#1C1C21` popover). No gradients, no glow.
- One accent color for links, primary button and selected item, used sparingly, not on every icon.
- Maximum 2 to 3 colored charts per screen, each in its own card. Never several mini-charts competing side by side.
- Maximum 3 to 4 colors per chart (never a pie with 6+ differently colored slices). Prefer desaturated tones over pure neon.
- Avoid: decorative gradients, glow on numbers or borders, several "No data" empty states visible at once, more than one KPI with the same visual weight.

## Hierarchy, shadow, spacing and grid

**Hierarchy in dashboards**
- KPI cards all the same size and weight is a sign of an AI-generated screen. Use one primary KPI (larger, full row: `grid-column: 1 / -1`, about 2.5rem, weight 700) and secondary KPIs at about 1.5rem.
- Do not reuse the same example delta (such as +12.5%) in several cards. Show only real variation.
- Exception: comparison screens (reconciliation), where numbers weigh the same. Hierarchy follows use there.

**Shadows**
- When everything has a shadow, nothing stands out. Resting card, input and table row use a 1px border or a slightly lighter background, no shadow.
- Shadow only on what floats above the page: dropdown, popover, modal, sheet, toast (for example `box-shadow: 0 8px 24px rgb(0 0 0 / 0.35)`).
- In dark theme shadows barely show. Use a lighter surface per level (background, card, popover).

**White space**
- Three kinds: micro (padding inside a button), macro (margins between sections), active (deliberately large gaps that pull the eye to what matters).
- Doubling the space (padding 16px to 32px, gap 8px to 16px) on the same content moves it from cramped to premium.

**Grid**
- A grid is a ratio system, not 12 equal columns: 4+8 creates hierarchy between main and side content, 6+6 is equal, 3+9 pushes everything to one side.
- At smaller breakpoints the grid reconfigures instead of disappearing (12 to 6 to 4 to 1). Every breakpoint is a grid decision.
- Gutter is a style choice: 8px feels tight and technical, 24px balanced, 40px editorial. Breaking the grid on purpose (a full-bleed image) is contrast, used sparingly.

## Numbers, tables and dashboards (desktop)

- Tabular numerals: `font-variant-numeric: tabular-nums` (test that the chosen font supports it).
- Numbers right-aligned, text left-aligned. Currency symbol outside the number column; align on decimal places, not on the symbol.
- Abbreviate large values (thousand, million) with `Intl.NumberFormat` using `notation: "compact"`, and keep the full value in the tooltip. **Decision:** this applies to KPIs and summaries only. In reconciliation, bank slips (boletos) and financial checking tables, always show the full value.
- Recent dates relative ("2 hours ago", `Intl.RelativeTimeFormat`), older dates absolute (`Intl.DateTimeFormat`).

## Tables and lists on mobile

- Restructure instead of shrinking. From 6 columns, keep 3 chosen by use (identity/name, value, state), not by left-to-right order. The ID goes first because nobody reads it on a phone.
- No horizontal scroll (it hides half the row). Stack two lines per record: name left and value right on line one; state and date on line two.
- Cards kill comparison. Keep a fixed slot for the value (top right, same in every card) with tabular numerals, so the card list still reads as an aligned column of numbers.
- No column headers in cards. Label only what is ambiguous ("Due Mar 4", not "Mar 4"). Money needs no label.
- Sorting becomes a control (headers disappear). Hidden columns are not deleted: tapping the row expands invoice ID, notes and actions inline (two lines expand in place). The full record opens in a bottom sheet. Never open a new page just to take a look.
- The breakpoint belongs to the table, not the screen: six columns need about 700px; below that they become cards. The same table inside a desktop side panel also becomes cards (use container queries, `@container`). Below about 640px each row becomes a card.
- **Decision:** line-by-line comparison screens (financial reconciliation) keep the table with horizontal scroll and a fixed first column, because cards lose the comparison.
- In narrow cards, large values must not wrap (a lone "+" sign on its own line). Keep the value on one line and reduce font size on mobile.
- Inputs below 16px make Safari zoom on tap. Use 16px or more on mobile and the right keyboard (`inputmode`, `type`).
- Touch targets of 44px on touch devices (`@media (pointer: coarse)`), keeping the dense layout for mouse.
- Fixed bottom bars respect the safe area (`viewport-fit=cover`, `padding-bottom: env(safe-area-inset-bottom)`) and leave space at the end of the content so the last row is not covered.

## Navigation and lists

- Keep one main navigation. Everything else goes under "More".
- List cards on mobile: name/identity as the title, secondary information below, value always in the same corner. Do not repeat the customer name on two lines of the same card.
- A side column of categories or filters cuts the main content on mobile. Turn it into horizontal chips with scroll at the top.
- Action buttons next to a search or filter field align with, and match the relative width of, the row above them, so they do not look like two different systems.

## Search and filter

- One search field per screen.
- Applied filters stay visible above the results (chips with ×), with "Clear all" to remove them at once. Removing a filter never reloads the page or restarts the search.
- Show only the 2 to 3 most used filters as chips under the search field. The rest go behind a "Filters (N)" button with an active-count.
- Complex filters on mobile open as a full-screen overlay with an "Apply" button. **Decision:** simple status chips apply immediately, with no "Apply".
- Empty result: say why, and offer an action ("Clear search").

## Bulk actions

- A fixed bar appears at the top only while something is selected, replacing the list header. No selection, no bar.
- Always show the counter ("3 selected"), including items off-screen when paginated, plus "Select all (N)" and one way to clear the whole selection.
- At most 3 visible actions; extra actions go in a "More" menu.
- On mobile the checkbox is always visible on the row, at least 44px (never hidden behind hover).
- **Decision (reversible vs destructive):** a reversible action (resolve) runs directly and shows "Undo". Deleting a single item runs directly with "Undo", no dialog. Deleting 2 or more always asks for confirmation stating the quantity ("Delete 3 tickets?") and has no undo.

## Row actions, context menus and hover on touch

- Never put an essential action, information or state only in hover.
- Row actions are always visible on touch, or inside a "⋯" menu with a 44px target. On desktop they may appear on hover, but must also appear on keyboard focus.
- Hover styles only inside `@media (hover: hover) and (pointer: fine)`. Laptops with touch and tablets with a mouse mix both.
- An icon without text gets a visible label. A tooltip is only a complement; essential explanation stays visible or sits in a tappable "ⓘ".
- **Menus:** group actions by intent with dividers (rename with duplicate, share with copy link); the destructive action alone at the bottom, with red text on that item only. A filled destructive button style appears only on the confirmation screen.
- A menu measures the available space before opening and flips up or mirrors left so it stays inside the viewport.
- Desktop keyboard: arrows move through the list, a letter jumps to the action, Esc closes one level (not the whole menu).
- **Decision (touch):** on mobile the same actions open in a bottom sheet, and the visible "⋯" is always the main path. Long press, if present, is an extra shortcut and never the only way to open the actions.

## Tabs

- The active indicator slides to the new tab instead of jumping. Content switches with fade out, a short pause, fade in, keeping the same height to avoid layout shift.
- Up to 8 tabs on one line without wrapping: horizontal scroll with edge fades showing there is more.
- Keyboard: arrows move between tabs, Home goes to the first, End to the last. The focus ring never uses the same color as the active state.
- **Decision:** kanban stages on mobile stay as horizontal scrolling tabs, one stage at a time. Do not convert to a bottom sheet above 5 tabs.

## Kanban

- On mobile, never scroll in two directions at once (column scrolls inside, screen scrolls sideways). Use horizontal tabs, one stage per view.
- Drag and drop: three simultaneous signals that a card is held (cursor changes, card lifts with shadow or scale, background dims). One signal is not enough.
- The destination shows where the card will land before it is dropped: an insertion line over a column, a highlight over a zone. Dropping without a preview is a blind drop.
- A structured kanban always snaps the card into a column. Free placement is a canvas pattern.
- A wrong drop is reversible: a toast "Moved to Done" with "Undo" for about 5 seconds.

## Modal vs sheet

- If the user needs to see the page while editing, use a sheet. If it needs a full stop (delete the account), use a modal with a scrim.
- A mobile sheet never has a single height: three snap points (peek = summary, half = form, full = long content). Released between two, it snaps up to the nearest.
- Close by release velocity, not distance: a fast flick closes from anywhere, a slow drag must pass the midline. Always include a visible close (X) for people who cannot drag.
- On desktop it becomes a side sheet: minimum 320px wide (the form breaks below) and maximum 40% of the screen (above it becomes a page). The list stays readable next to it.
- Never stack two sheets. Tapping a saved item inside a sheet replaces the content and adds a back arrow.
- Accessibility: `role="dialog"` with `aria-modal`, focus trap, Esc closes, focus returns to the element that opened it.

## Buttons and forms

- A disabled (gray) button is bad: it leaves the tab order, screen readers do not announce it, gray on gray fails contrast, and its tooltip never fires (disabled elements get no pointer events).
- Prefer an active button: on click, validate, highlight the blocking fields and move focus to the first one. Implement with `aria-disabled="true"` plus a JS click block, `aria-invalid` and `aria-describedby` on the field.
- Disabled and loading are different. While a request runs the button keeps focus, shows a spinner with a fixed width and sets `aria-busy="true"`, without turning gray.
- Disabled is still valid when a prerequisite lives on another page, for "Save" with no changes, and for type-to-confirm. Then explain the reason on screen, use `aria-disabled`, and open the tooltip on tap/click, not only on hover.

**Validation**
- Validate when the user leaves the field (blur), not on every keystroke and not only on submit.
- A field that has already shown an error switches to live validation and clears the message the moment the value is correct. Never make the user submit again to find out they are forgiven.
- Validation on button click stays as a safety net for fields never touched.
- **Decision:** no green check on every correct field (it repeats information that does not vary). Confirm visually only where correctness is not obvious (validated CPF/CNPJ, ZIP code found). Never rely on color alone: use an icon plus text for screen readers.

**Long and multi-step forms**
- Up to about 6 fields stays on a single screen.
- Desktop: one page with collapsible sections (people who register every day want to see everything).
- Mobile above about 6 fields: steps of 2 to 5 fields, named ("Data → Address → System"), with "Step X of N" and a Back button always available.
- Validate and show errors per step, near the field. Save a draft automatically at each step and allow resuming.
- Do not use lead-capture tactics (asking for sensitive data last because of sunk cost) in internal forms.

## Upload

- The drop zone responds visually to a file being dragged (border, glow, text) before it is dropped. Toggle a CSS class on `dragenter`, `dragover`, `dragleave`, `drop`.
- Progress shows percentage and time remaining, not a generic spinner (`XMLHttpRequest.upload.onprogress` or `fetch` with a `ReadableStream`).
- If an upload fails near the end, keep the file and allow resuming with one tap (inline retry, keep the `File` in memory).
- Received-file feedback shows thumbnail, type and size, not just the name (`URL.createObjectURL`).
- Multiple files: each has its own progress and retry, and one failure does not block the others (`Promise.allSettled`).

## States, confirmation and errors

**Empty state**
- Say what the screen is for, why it is empty and what to do next. Never leave a dead end.
- One clear action that fills the screen (sources disagree on primary vs secondary button; do not pin the button type).
- Illustration only if there is room; avoid several illustrated empty states on one screen.
- Treat separately: first use (nothing created), no search results (offer "clear search"), and task completed.

**Destructive confirmation (friction ladder)**
- No confirmation, simple confirmation, explicit consequence, type to confirm. "Are you sure?" on everything trains people to click without reading.
- Prefer undo (toast for about 8 seconds, soft delete with `deleted_at`) over confirmation when the action is reversible.
- Reserve typing the resource name for the serious (delete the account, wipe the database, mass send). Initial focus on Cancel for destructive actions.
- Dialog text is specific to the action and says whether it is irreversible.
- Async confirmation: success closes the dialog and shows a toast; an error keeps the dialog open with an inline message.

**Network/API errors in forms**
- Separate "we could not save" (try again) from "this field is wrong" (fix it).
- Preserve everything typed. Never clear the form after an error. Offer a retry button.
- Never fail silently.
- Specific, human message that says what to do. "Something went wrong" is not an error message.
- Accessibility: move focus to the error summary or the first field in error, `role="alert"` or `aria-live` for dynamic messages, `aria-describedby` linking error to field.
- Test error paths as carefully as the happy path.

## Optional style: dense "Linear-like" (desktop, keyboard)

Use only when the product asks for it. It is not a universal rule, and it is not for touch or POS screens (13px text and low-contrast borders fail there).

- Density: 13px text, 32px rows, 1% tighter letter spacing.
- No shadows: 1px border at about 8% white, depth through 3 background values, hover changes the surface tone.
- One accent color (indigo) only on the selected row and primary button; status by icon, not colored pill; everything else gray.
- Every action shows its keyboard shortcut. Hover responds in about 80ms, transitions under 150ms, no bounce or overshoot.
- 4px grid, 16px icons centered with the text, labels left and numbers right, nothing centered.

## Examples

Screens already reviewed with these rules:

- **Dashboard V2 / Info Intelligence (owner dashboard):** cut repeated text, reduced the colors of the payment donut, fixed KPI hierarchy and the line chart.
- **Tickets (Atendimentos) in Sistema Info:** removed triplicated navigation, removed "Medium"/"WhatsApp" repeated on every row, fixed contrast of "Resolved" vs "New".
- **Opportunities (kanban) in Sistema Info:** fixed misaligned card grid, removed "NO REGISTRATION"/"Cold" repeated on every card, removed button glow, fixed two-direction scrolling.
- **Knowledge in Sistema Info:** side categories became horizontal chips on mobile, aligned search and filter buttons.

## Sources and confidence

- Hierarchy, shadows, context menus, tabs, validation, kanban drag and drop, grid and white space come from short videos of one account (designmotionhq, tied to a paid product). Treat them as one source's opinion, not area consensus.
- Search/filter, bulk actions, empty/confirmation/error states and the button caveat were cross-checked against independent sources (for example Algolia, LogRocket, Kodeco, UX Collective, Eleken, HashiCorp Helios, SAP Fiori, Carbon, NN/g).
- Long forms come from form-builder vendors focused on lead generation and were adapted to internal systems. Hover on touch comes from weaker but consistent sources.
- Anything marked **Decision** reflects a choice by the owner of the systems and overrides a conflicting source.

## Known gaps

- Hierarchy/shadow video: only tells 03 and 04 of 5 were captured.
- Still no dedicated rules for charts beyond the Dashboard V2 direction, notifications and toasts beyond undo, or onboarding.
