# HTML & CSS Standards

Standards for HTML and CSS in ecosystem web projects.

## HTML Standards

### Document Structure

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta name="description" content="Page description">
    <title>Page Title</title>
    <link rel="stylesheet" href="/css/main.css">
</head>
<body>
    <header>
        <nav aria-label="Main navigation">
            <!-- Navigation -->
        </nav>
    </header>

    <main>
        <!-- Main content -->
    </main>

    <footer>
        <!-- Footer content -->
    </footer>

    <script src="/js/main.js" defer></script>
</body>
</html>
```

### Semantic Elements

Use semantic HTML5 elements:

| Element | Use Case |
|---------|----------|
| `<header>` | Page or section header |
| `<nav>` | Navigation links |
| `<main>` | Main content (one per page) |
| `<article>` | Self-contained content |
| `<section>` | Thematic grouping |
| `<aside>` | Tangentially related content |
| `<footer>` | Page or section footer |

### Accessibility

```html
<!-- Images require alt text -->
<img src="logo.png" alt="Company logo">

<!-- Decorative images use empty alt -->
<img src="decoration.png" alt="">

<!-- Form labels are required -->
<label for="email">Email</label>
<input type="email" id="email" name="email" required>

<!-- Use ARIA when needed -->
<button aria-label="Close dialog" aria-expanded="false">
    <svg>...</svg>
</button>

<!-- Skip link for keyboard users -->
<a href="#main-content" class="skip-link">Skip to main content</a>
```

### Formatting

- 2 spaces for indentation
- Lowercase element names and attributes
- Double quotes for attribute values
- Self-closing tags without slash (`<img>`, `<br>`, `<input>`)
- Boolean attributes without values (`disabled`, `required`, `checked`)

```html
<!-- Good -->
<input type="email" id="email" required>
<img src="photo.jpg" alt="Description">

<!-- Avoid -->
<INPUT TYPE='email' ID='email' required="required" />
<img src="photo.jpg" alt="Description" />
```

## CSS Standards

### File Organization

```
css/
├── base/
│   ├── reset.css       # CSS reset/normalize
│   ├── typography.css  # Font definitions
│   └── variables.css   # CSS custom properties
├── components/
│   ├── buttons.css
│   ├── cards.css
│   └── forms.css
├── layouts/
│   ├── grid.css
│   ├── header.css
│   └── footer.css
├── pages/
│   └── home.css
└── main.css            # Import all
```

### CSS Custom Properties

```css
:root {
    /* Colors */
    --color-primary: #0066cc;
    --color-secondary: #6c757d;
    --color-success: #28a745;
    --color-error: #dc3545;
    --color-warning: #ffc107;

    /* Typography */
    --font-family-sans: system-ui, -apple-system, sans-serif;
    --font-family-mono: 'Fira Code', monospace;
    --font-size-base: 1rem;
    --line-height-base: 1.5;

    /* Spacing */
    --spacing-xs: 0.25rem;
    --spacing-sm: 0.5rem;
    --spacing-md: 1rem;
    --spacing-lg: 2rem;
    --spacing-xl: 4rem;

    /* Borders */
    --border-radius: 4px;
    --border-width: 1px;

    /* Shadows */
    --shadow-sm: 0 1px 2px rgba(0, 0, 0, 0.1);
    --shadow-md: 0 4px 6px rgba(0, 0, 0, 0.1);
    --shadow-lg: 0 10px 15px rgba(0, 0, 0, 0.1);

    /* Transitions */
    --transition-fast: 150ms ease;
    --transition-normal: 300ms ease;
}
```

### Naming Convention (BEM)

```css
/* Block */
.card { }

/* Element */
.card__header { }
.card__body { }
.card__footer { }

/* Modifier */
.card--featured { }
.card--compact { }

/* State */
.card.is-active { }
.card.is-loading { }
```

### Formatting

```css
/* 2 space indentation */
/* One selector per line */
/* Space after colon */
/* Semicolon on every declaration */

.button,
.btn {
    display: inline-flex;
    align-items: center;
    padding: var(--spacing-sm) var(--spacing-md);
    font-size: var(--font-size-base);
    border-radius: var(--border-radius);
    transition: background-color var(--transition-fast);
}

.button:hover,
.btn:hover {
    background-color: var(--color-primary-dark);
}
```

### Property Order

Group properties by type:

```css
.element {
    /* Positioning */
    position: relative;
    top: 0;
    right: 0;
    z-index: 1;

    /* Display & Box Model */
    display: flex;
    flex-direction: column;
    width: 100%;
    padding: 1rem;
    margin: 0;
    border: 1px solid #ccc;

    /* Typography */
    font-family: var(--font-family-sans);
    font-size: 1rem;
    line-height: 1.5;
    color: #333;

    /* Visual */
    background-color: #fff;
    border-radius: 4px;
    box-shadow: var(--shadow-sm);

    /* Animation */
    transition: opacity 0.3s ease;
}
```

### Responsive Design

```css
/* Mobile-first approach */
.container {
    padding: var(--spacing-sm);
}

/* Tablet */
@media (min-width: 768px) {
    .container {
        padding: var(--spacing-md);
    }
}

/* Desktop */
@media (min-width: 1024px) {
    .container {
        max-width: 1200px;
        margin: 0 auto;
        padding: var(--spacing-lg);
    }
}
```

### Dark Mode

```css
/* Light mode (default) */
:root {
    --color-bg: #ffffff;
    --color-text: #333333;
}

/* Dark mode */
@media (prefers-color-scheme: dark) {
    :root {
        --color-bg: #1a1a1a;
        --color-text: #e0e0e0;
    }
}

/* Manual toggle */
[data-theme="dark"] {
    --color-bg: #1a1a1a;
    --color-text: #e0e0e0;
}
```

## Tools

### Linting

- **HTMLHint**: HTML linting
- **Stylelint**: CSS linting

```json
// .stylelintrc.json
{
    "extends": ["stylelint-config-standard"],
    "rules": {
        "indentation": 2,
        "string-quotes": "double",
        "no-duplicate-selectors": true,
        "selector-class-pattern": "^[a-z][a-z0-9]*(-[a-z0-9]+)*(__[a-z0-9]+(-[a-z0-9]+)*)?(--[a-z0-9]+(-[a-z0-9]+)*)?$"
    }
}
```

### Formatting

- **Prettier**: Code formatting

```json
// .prettierrc
{
    "printWidth": 100,
    "tabWidth": 2,
    "useTabs": false,
    "singleQuote": false,
    "htmlWhitespaceSensitivity": "css"
}
```

## Performance

- Minimize CSS file size
- Use CSS custom properties instead of preprocessor variables
- Avoid deep nesting (max 3 levels)
- Use efficient selectors (avoid universal selector)
- Lazy load non-critical CSS

```html
<!-- Critical CSS inline -->
<style>
    /* Above-the-fold styles */
</style>

<!-- Non-critical CSS deferred -->
<link rel="preload" href="/css/main.css" as="style" onload="this.rel='stylesheet'">
```
