# Accessibility Improvements for index.html

This document outlines recommended accessibility improvements for the `index.html` file. Each section details the impact of the issue, describes the current problem, and suggests how to fix it. Styling changes related to these fixes (e.g., visible focus indicators) should be added to `index.css`.

## 1. Semantic HTML & Structure

### 1.1. Overuse of Generic `<div>` and `<span>` Elements
*   **Impact:** Using non-semantic elements makes it harder for assistive technologies (like screen readers) to understand the structure and purpose of different sections of the page. This can make navigation and comprehension difficult for users relying on these technologies.
*   **Problem:** Many sections are built with generic `<div>` elements where more specific HTML5 semantic tags would be appropriate (e.g., `<div class="main">`, various `Component__row` divs).
*   **Recommendation:**
    *   Replace `<div class="main">` with `<main class="main">`.
    *   Evaluate `div` elements with classes like `Component`, `Column`, `Hero__slide-body`, etc., to see if they can be replaced by `<section>`, `<article>`, `<aside>`, or `<nav>` where appropriate. For example, a block of related content introduced by a heading is often a good candidate for `<section>`.
    *   Example:
        ```html
        <!-- Before -->
        <div class="main">
          <!-- ... -->
        </div>

        <!-- After -->
        <main class="main">
          <!-- ... -->
        </main>
        ```

### 1.2. Defining Navigation Regions
*   **Impact:** Without clear navigation landmarks, users of assistive technologies may have difficulty quickly finding and understanding the purpose of different navigation blocks (main navigation, secondary navigation, footer navigation).
*   **Problem:** Navigation menus (e.g., `ul` with `id="menu-huvudnavigering"`, `id="menu-sekundar-navigering"`, and footer menus) are not wrapped in `<nav>` elements.
*   **Recommendation:** Wrap primary, secondary, and footer navigation lists in `<nav>` elements and provide an `aria-label` to distinguish them if there are multiple `<nav>` elements on the page.
    *   Example:
        ```html
        <!-- Before -->
        <div class="header__navigation d-none d-lg-flex">
          <ul id="menu-huvudnavigering" class="main-nav">
            <!-- ... -->
          </ul>
          <ul id="menu-sekundar-navigering" class="secondary-nav">
            <!-- ... -->
          </ul>
        </div>

        <!-- After -->
        <div class="header__navigation d-none d-lg-flex">
          <nav aria-label="Main navigation">
            <ul id="menu-huvudnavigering" class="main-nav">
              <!-- ... -->
            </ul>
          </nav>
          <nav aria-label="Secondary navigation">
            <ul id="menu-sekundar-navigering" class="secondary-nav">
              <!-- ... -->
            </ul>
          </nav>
        </div>
        ```
        Apply similar `<nav aria-label="Footer links">` or more specific labels in the footer.

### 1.3. Heading Hierarchy
*   **Impact:** An illogical or skipped heading hierarchy can confuse screen reader users, as they often navigate by headings to understand the page structure and find content quickly.
*   **Problem:** Heading levels (e.g., `<h5>` for "Arbetsgivarguiden" directly after an `<h1>`, potential skips) are not always sequential or logically structured.
*   **Recommendation:**
    *   Ensure headings follow a logical order (H1 -> H2 -> H3, etc.) without skipping levels.
    *   Re-evaluate the use of `<h5>` in `Component__guide-teaser`. If these are significant sections following the main `<h1>`, they should likely be `<h2>`.
    *   Review all `Texteditor__title` instances (currently `<h2>`) to confirm they are appropriate sub-sections of the main page `<h1>` or the section they reside in.
    *   Example (`Component__guide-teaser`):
        ```html
        <!-- Before -->
        <h5 class="no-border-bottom">Arbetsgivarguiden</h5>

        <!-- After (assuming it's a main section) -->
        <h2 class="no-border-bottom">Arbetsgivarguiden</h2>
        ```

## 2. Image Accessibility

### 2.1. Missing or Generic `alt` Text
*   **Impact:** Users who cannot see images (e.g., blind users, users with low bandwidth, or if an image fails to load) will miss out on the information conveyed by the image if appropriate `alt` text is not provided.
*   **Problem:**
    *   Several images in the Hero slider (after the first) have `alt=""`.
    *   The banner image `Avtal-25-Banner_nyhetsbrev_nyttavtal-1140x570.png` has `alt=""`.
    *   Social media icons in the footer have non-descriptive `alt` text (e.g., filenames).
*   **Recommendation:**
    *   For informative images: Provide concise, descriptive `alt` text that conveys the meaning or purpose of the image.
        *   Example Hero Slide: `alt="Happy staff member assisting a guest at a hotel reception"`
        *   Example Banner: If the banner contains text, that text should be in the `alt` attribute or the image should be accompanied by actual text.
    *   For decorative images: If an image is purely decorative and adds no informational value, use `alt=""`. This is already done for some, but double-check the intent for each.
    *   For social media icons: Use descriptive `alt` text.
        ```html
        <!-- Before (example) -->
        <img src=".../logo-facebook@2x.png" class="img-fluid" />

        <!-- After -->
        <img src=".../logo-facebook@2x.png" class="img-fluid" alt="Visita on Facebook" />
        ```
        (And similarly for X, YouTube, Instagram)

## 3. Interactive Elements & Forms

### 3.1. Vague Link Text
*   **Impact:** Vague link text like "Läs mer" or "Klicka här" makes it difficult for screen reader users navigating via a list of links to understand the destination or purpose of the link without its surrounding context.
*   **Problem:** Links such as "Läs mer här!" (lines 594, 616) and "Till Arbetsgivarguiden" (line 535) could be more descriptive.
*   **Recommendation:** Make link text more descriptive so it's clear where the link leads, even out of context.
    *   Alternatively, use `aria-label` to provide a more descriptive accessible name if changing the visible text is not desired.
    *   Example:
        ```html
        <!-- Before -->
        <a class="Texteditor__link" data-password="true" href="https://visita.se/swedbank-pay/" target="_blank">
          Läs mer här!
        </a>

        <!-- After (Option 1: Visible text change) -->
        <a class="Texteditor__link" data-password="true" href="https://visita.se/swedbank-pay/" target="_blank">
          Läs mer om Swedbank Pay kortinlösen
        </a>

        <!-- After (Option 2: aria-label) -->
        <a class="Texteditor__link" data-password="true" href="https://visita.se/swedbank-pay/" target="_blank" aria-label="Läs mer om Swedbank Pay kortinlösen">
          Läs mer här!
        </a>
        ```

### 3.2. Links Opening New Tabs
*   **Impact:** Users may not be aware that a link will open in a new tab or window, which can be disorienting, especially for users with cognitive disabilities or screen magnifiers.
*   **Problem:** Links with `target="_blank"` do not visually or audibly indicate this behavior.
*   **Recommendation:** Indicate that a link opens in a new tab/window. This can be done by:
    *   Adding text like "(opens in new window)" to the link text.
    *   Adding an icon with appropriate `alt` text (e.g., `<img src="new-window-icon.png" alt="(opens in new window)">`).
    *   Using an `aria-label` to provide this information if visual changes are not preferred.
    *   Example:
        ```html
        <a href="https://example.com" target="_blank">Visit Example.com <span class="visually-hidden">(opens in new window)</span></a>
        ```
        (Requires a `visually-hidden` class in `index.css` to hide the span visually but keep it for screen readers).

### 3.3. Lock Icon Indication
*   **Impact:** While the lock icon visually indicates restricted content, this meaning isn't conveyed to screen reader users if the icon itself isn't accessible.
*   **Problem:** Links with `<i class="fo-icon-lock"></i>` rely solely on the visual icon for meaning.
*   **Recommendation:** Ensure the "restricted" meaning is conveyed to screen readers.
    *   Add visually hidden text next to the icon: `Till Kassan <i class="fo-icon-lock"></i> <span class="visually-hidden">(Restricted content, members only)</span>`
    *   Add an `aria-label` to the link itself that includes this information.
    *   If the icon is an SVG, it could have a `<title>` element.
    *   Example:
        ```html
        <a class="nav-link" href="https://visita.se/nyhetsbrev/" aria-label="Nyhetsbrev (members only)">Nyhetsbrev<i class="fo-icon-lock" aria-hidden="true"></i></a>
        ```
        Or ensure the icon itself has an accessible name if it's an `<img>` or an SVG with a `<title>`. If it's just an `<i>` tag with a class, use `aria-hidden="true"` on the icon and provide the description on the parent link or with a visually-hidden span.

### 3.4. Non-Button Elements Used as Buttons
*   **Impact:** Elements like `<a>` or `<div>` styled to look like buttons but lacking button semantics are not announced as buttons by screen readers and may not be keyboard operable as expected.
*   **Problem:**
    *   Search overlay toggle: `<a data-toggle="search-overlay" ...>`
    *   Mobile menu toggle: `<a href="index.html#" data-toggle="menu" ...>`
    *   Search overlay close: `<div data-toggle="search-overlay" class="Search-overlay__close">...</div>`
    *   Slider navigation: `<button data-button="prev" class="external__button fo-icon-chevron"></button>` (these are buttons but lack accessible names).
*   **Recommendation:**
    *   For elements that trigger actions (like opening/closing overlays or menus): Use actual `<button>` elements.
    *   If `<a>` tags must be used (e.g., for specific styling reasons and they don't navigate), add `role="button"` and ensure they are keyboard operable (e.g., respond to Enter/Space).
    *   Provide accessible names for all button-like controls, especially icon-only buttons.
    *   Example for search overlay close:
        ```html
        <!-- Before -->
        <div data-toggle="search-overlay" class="Search-overlay__close"><i class="fo-icon-close"></i></div>

        <!-- After -->
        <button type="button" data-toggle="search-overlay" class="Search-overlay__close" aria-label="Close search">
          <i class="fo-icon-close" aria-hidden="true"></i>
        </button>
        ```
    *   Example for slider buttons:
        ```html
        <!-- Before -->
        <button data-button="prev" class="external__button fo-icon-chevron"></button>

        <!-- After -->
        <button data-button="prev" class="external__button fo-icon-chevron" aria-label="Previous slide"></button>
        ```
        (Add `aria-hidden="true"` to the `<i>` icon if it's purely decorative and the `aria-label` is on the button).

### 3.5. Form Controls Missing Labels
*   **Impact:** Without properly associated `<label>` elements, users of assistive technologies may not understand the purpose of form inputs. Placeholder text is not a reliable substitute for labels.
*   **Problem:** Search inputs (`<input class="search-field">` and `<input class="search form-control">`) use `placeholder` attributes but lack associated `<label>` elements.
*   **Recommendation:** Add a `<label>` for every form input and associate it using `for` and `id` attributes. If a visible label is not desired for design reasons, use a visually-hidden label.
    *   Example:
        ```html
        <!-- Before -->
        <input class="search-field" name="s" type="search" placeholder="Sök..." value="" autocomplete="off" />

        <!-- After (with visually-hidden label) -->
        <label for="search-main" class="visually-hidden">Search</label>
        <input id="search-main" class="search-field" name="s" type="search" placeholder="Sök..." value="" autocomplete="off" />
        ```
        (Requires a `visually-hidden` class in `index.css`).

## 4. ARIA Usage

### 4.1. ARIA for Dynamic Content (Dropdowns, Sliders)
*   **Impact:** Without proper ARIA attributes, dynamic content changes (like expanding menus or changing slides) may not be announced to screen reader users, making the interface confusing and difficult to use.
*   **Problem:**
    *   Navigation dropdowns use `aria-haspopup` and `aria-expanded`, which is good, but ensure the `ul` has `role="menu"` and `li` has `role="menuitem"`.
    *   Hero slider and External content slider lack ARIA attributes to describe their role as carousels and announce slide changes.
*   **Recommendation:**
    *   **Dropdown Menus:**
        *   Ensure the `<ul>` that forms the dropdown menu has `role="menu"`.
        *   Ensure each `<li>` within that menu has `role="menuitem"` (or `role="none"` if it's just a separator, and its contained link has `role="menuitem"`).
        *   JavaScript must toggle `aria-expanded` on the controlling button (`<a>` or `<button>`).
    *   **Sliders/Carousels (Hero & External):**
        *   The main container for the slider should have `aria-roledescription="carousel"`.
        *   Individual slides could have `role="group"` and `aria-label="Slide X of Y"`.
        *   Use an `aria-live` region (e.g., `aria-live="polite"`) to announce when a slide changes (e.g., "Slide 2 of 5 displayed").
        *   Ensure slider controls (prev/next buttons) have `aria-label`s (as mentioned above) and potentially `aria-controls` pointing to the ID of the slide container.

## 5. Keyboard Navigation & Focus Management

### 5.1. Visible Focus Indicator
*   **Impact:** Users who rely on keyboard navigation need a clear visual indication of which element currently has focus. If this is missing or too subtle, navigation becomes very difficult.
*   **Problem:** The default browser focus indicator might be suppressed or not sufficiently visible due to CSS resets or custom styling.
*   **Recommendation:** Ensure all interactive elements (links, buttons, form inputs) have a highly visible focus indicator. This can be styled in `index.css`.
    *   Example CSS in `index.css`:
        ```css
        /* Basic example - customize to match site design */
        *:focus {
          outline: 2px solid blue !important; /* Or a color that fits your design and has good contrast */
          outline-offset: 2px;
        }
        /* For elements that might have default outline removed */
        a:focus, button:focus, input:focus, select:focus, textarea:focus {
            outline: 2px solid blue !important;
            outline-offset: 2px;
        }
        ```
        Be mindful of `:focus-visible` for a more modern approach that only shows focus rings for keyboard users.

### 5.2. Logical Focus Order
*   **Impact:** An illogical focus order (e.g., jumping around the page unpredictably) can be extremely confusing for keyboard users.
*   **Problem:** While not explicitly identified as broken, complex layouts can sometimes lead to a focus order that doesn't match the visual flow if not managed.
*   **Recommendation:** Test keyboard navigation thoroughly. The focus order should generally follow the visual reading order. Avoid using `tabindex` with positive values. If `tabindex="-1"` is used to make non-interactive elements focusable programmatically, ensure it's done for a good reason.

### 5.3. Modal/Overlay Focus Management (Search Overlay)
*   **Impact:** When a modal or overlay is active, keyboard focus should be trapped within it. If not, users can accidentally tab to elements obscured by the overlay, leading to confusion.
*   **Problem:** The Search Overlay (`<div class="Search-overlay">`) needs focus trapping.
*   **Recommendation:**
    *   When the search overlay becomes visible, focus should be moved to an element within it (e.g., the search input).
    *   Keyboard focus (Tab and Shift+Tab) should be contained within the overlay.
    *   The "Escape" key should close the overlay.
    *   When the overlay is closed, focus should return to the element that opened it (e.g., the search icon/button).
    *   This usually requires JavaScript.

### 5.4. Skip Links
*   **Impact:** For pages with many navigation links before the main content, keyboard users have to tab through all of them to reach the content. A skip link provides a shortcut.
*   **Problem:** No "Skip to main content" link is present.
*   **Recommendation:** Add a "Skip to main content" link as the very first focusable element in the `<body>`. This link can be visually hidden until it receives focus. It should link to the ID of the main content container.
    *   Example HTML:
        ```html
        <body>
          <a href="#main-content" class="skip-link">Skip to main content</a>
          <!-- ... rest of the body ... -->
          <main id="main-content" class="main">
            <!-- ... -->
          </main>
        </body>
        ```
    *   Example CSS in `index.css`:
        ```css
        .skip-link {
          position: absolute;
          top: -40px; /* Hide off-screen */
          left: 0;
          background: #000; /* Example styling */
          color: white;
          padding: 8px;
          z-index: 10000; /* Ensure it's on top */
        }
        .skip-link:focus {
          top: 0; /* Bring into view on focus */
        }
        ```

## 6. Color Contrast

*   **Impact:** Insufficient color contrast between text and its background makes content difficult or impossible to read for users with low vision or color blindness.
*   **Problem:** 
    *   The actual color contrast ratios are unknown without rendering the page and using a color contrast checker. Classes like `theme-color-green`, `theme-color-blue` need to be checked in general.
    *   **Specific Issue Identified:** The selector `span.Texteditor__label.post-content__label.post-label.inherit-parent-shadow-color` has been reported to have low-contrast between its foreground (text) and background colors.
*   **Recommendation:** 
    *   Use a color contrast checking tool (many available online or as browser extensions) to verify that all text/background color combinations meet at least WCAG AA guidelines (4.5:1 for normal text, 3:1 for large text [18pt or 14pt bold]). Adjust colors in `index.css` as needed. This also applies to text on background images.
    *   **For `span.Texteditor__label...`:** Investigate the specific colors being applied to elements matching this selector. Adjust the text color, background color, or both in `index.css` to meet the required contrast ratio.

## 7. Icons (Icon Fonts/SVGs)

*   **Impact:** If icons that convey meaning are not announced to screen readers, users miss that information. Decorative icons, if not hidden, can add audible clutter.
*   **Problem:** Icons (e.g., `fo-icon-search`, `fo-icon-chevron-down`) are implemented using `<i>` tags with CSS classes. Their accessibility depends on how they are used in context.
*   **Recommendation:**
    *   **Informative Icons:** If an icon conveys meaning and is not accompanied by visible text (e.g., an icon-only button), it needs an accessible name.
        *   For `<i>` tag icons, this is often best achieved by adding an `aria-label` to the parent button or link, or by including a visually-hidden `<span>` with descriptive text.
        *   Example: `<button aria-label="Search"><i class="fo-icon-search" aria-hidden="true"></i></button>`
    *   **Decorative Icons:** If an icon is purely decorative or redundant with adjacent visible text, hide it from assistive technologies using `aria-hidden="true"`.
        *   Example: `<span>Menu <i class="fo-icon-chevron-down" aria-hidden="true"></i></span>` (assuming "Menu" is sufficient).
    *   Many icons are already next to text (e.g., `<i class="fo-icon-user"></i>Logga in`). In these cases, the text "Logga in" provides the accessible name, and the icon can be considered decorative (`aria-hidden="true"` on the `<i>` tag would be appropriate).

## 8. Touch Target Size and Spacing

*   **Impact:** Small touch targets or targets that are too close together can be difficult for users with motor impairments to activate accurately. This can also affect users on small screens or those with larger fingers.
*   **Problem:** The following touch targets have been identified as potentially too small or inadequately spaced:
    *   Search button in the main search overlay: `div.typeahead__field > div.typeahead__query > div.typeahead__button > button`
        *   Associated HTML: `<button type="submit">`
    *   Search input in the member search block: `div.typeahead__container > div.typeahead__field > div.typeahead__query > input.search`
        *   Associated HTML: `<input class="search form-control box-shadow-sm" name="search-member-q" type="search" ...>`
*   **Recommendation:**
    *   **Increase Size:** Ensure that all touch targets are at least 44x44 CSS pixels in size. This can be achieved by increasing padding, width, or height of the elements via `index.css`.
    *   **Ensure Spacing:** If multiple touch targets are close to each other, ensure there is adequate spacing around them to prevent accidental activation of an adjacent target.
    *   **Check WCAG Guidelines:** Refer to WCAG 2.1 Success Criterion 2.5.5 Target Size (Level AAA) for best practices, although aiming for at least 44x44px is a good general rule and closer to Level AA requirements in WCAG 2.2.
    *   **For the identified elements:** Review their current dimensions using browser developer tools. Adjust their CSS in `index.css` to increase their clickable area. For example:
        ```css
        /* Example for the search button */
        .typeahead__button button {
          padding: 10px; /* Adjust as needed */
          min-width: 44px;
          min-height: 44px;
          box-sizing: border-box; /* Important if adding padding to elements with set width/height */
        }

        /* Example for the search input */
        .block-search-member__form .search.form-control {
          padding-top: 10px; /* Adjust as needed */
          padding-bottom: 10px; /* Adjust as needed */
          min-height: 44px;
          box-sizing: border-box;
        }
        ```
    *   Visually inspect the changes to ensure they don't negatively impact the layout while improving target size.
