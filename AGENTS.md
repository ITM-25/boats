# HTML Page Creation Specifications

## Overview
This document provides complete specifications for creating HTML pages in this project. All pages must follow the template structure and styling defined in `/template/`.

## Template Analysis

### Available Templates
1. **template.html** - Basic article layout with left sidebar for project navigation
2. **sidebar.html** - Article layout with right sidebar for page navigation (wrapped in `.page-container`)

### Core Structure

#### Required HTML Boilerplate
```html
<!doctype html>
<html lang="en">
    <head>
        <meta charset="UTF-8" />
        <meta name="viewport" content="width=device-width, initial-scale=1.0" />
        <title>[Page Title] · [Site Name]</title>
        <meta name="description" content="[Page description]" />
        <link rel="icon" href="../img/favicon.png" type="image/png" />
        <link rel="stylesheet" href="css/style.css" />
    </head>
    <body>
        <!-- Content here -->
    </body>
</html>
```

## Page Layouts

### Layout 1: Basic Article with Left Sidebar (Projects)

**Use case:** Main content pages with project navigation

**Structure:**
```html
<body>
    <!-- Header -->
    <header></header>

    <!-- Main Content -->
    <article>
        <h1>[Page Title]</h1>
        <p class="date">[Date]</p>
        
        <section class="content">
            <!-- Content here -->
        </section>
    </article>

    <!-- Left Sidebar -->
    <aside class="floating-sidebar-left">
        <h3>Projects</h3>
        <nav class="sidebar-nav-left">
            <a href="[path]">[Project Name]</a>
            <!-- More links -->
        </nav>
    </aside>
</body>
```

### Layout 2: Article with Right Sidebar (Pages)

**Use case:** Content pages with internal page navigation

**Structure:**
```html
<body>
    <!-- Header -->
    <header></header>

    <!-- Page Container -->
    <div class="page-container">
        <!-- Main Content -->
        <article>
            <h1>[Page Title]</h1>
            <p class="date">[Date]</p>
            
            <section class="content">
                <!-- Content here -->
            </section>
        </article>

        <!-- Floating Sidebar -->
        <aside class="floating-sidebar">
            <h3>Pages</h3>
            <nav class="sidebar-nav">
                <a href="[path]">[Page Name]</a>
                <a href="[path]" class="active">[Current Page]</a>
                <!-- More links -->
            </nav>
        </aside>
    </div>
</body>
```

## Content Elements

### Typography

#### Headings
```html
<h1>Main Page Title</h1>           <!-- 36px, font-weight: 600 -->
<h2 id="section-id">Section</h2>   <!-- 24px, font-weight: 600 -->
<h3 id="subsection">Subsection</h3> <!-- 20px, font-weight: 600 -->
```

**Rules:**
- Always include `id` attributes on h2 and h3 for anchor links
- Use kebab-case for IDs (e.g., `id="get-started"`)

#### Date Display
```html
<p class="date">January 23, 2026</p>
```

#### Paragraphs
```html
<p>Regular paragraph text with <strong>bold text</strong> for emphasis.</p>
```

### Code Elements

#### Inline Code
```html
<p>Use <code>command name</code> to execute.</p>
```

#### Code Blocks
```html
<pre><code>command line example
multiple lines supported</code></pre>
```

#### Code with Syntax Highlighting
```html
<pre><code><span class="comment"># This is a comment</span>
command --flag <span class="parameter">value</span></code></pre>
```

**Available classes:**
- `.comment` - Gray color (#6b7280) for comments
- `.parameter` - Red color (#dc2626) for parameters/flags

### Lists

#### Unordered Lists
```html
<ul>
    <li>First item</li>
    <li>Second item</li>
    <li>Third item</li>
</ul>
```

### Links

#### Standard Links
```html
<a href="https://example.com">Link Text</a>
```

**Rules:**
- Links automatically underline on hover
- Use relative paths for internal links
- External links should use full URLs

### Images

#### Standard Images
```html
<img src="img/image-name.png" alt="Descriptive alt text" />
```

**Rules:**
- Images auto-center with 24px top/bottom margin
- 16px border-radius applied automatically
- Max-width: 100% (responsive)
- Store images in `img/` directory relative to the HTML file

## Sidebar Specifications

### Left Sidebar (Projects Navigation)

**Purpose:** Navigate between different projects

**Classes:**
- Container: `.floating-sidebar-left`
- Navigation: `.sidebar-nav-left`

**Positioning:**
- Fixed position at `top: 80px`
- Left side of content
- Width: 240px
- Only visible on desktop (1024px+)

**Link behavior:**
- Default: Gray (#737373)
- Hover: Orange background (#ff6600), white text
- Active: Black text (#000), font-weight: 500

### Right Sidebar (Page Navigation)

**Purpose:** Navigate between pages within the same section

**Classes:**
- Container: `.floating-sidebar`
- Navigation: `.sidebar-nav`

**Positioning:**
- Fixed position at `top: 80px`
- Right side of content
- Width: 240px
- Only visible on desktop (1024px+)

**Link behavior:**
- Same as left sidebar
- Add `class="active"` to current page link

## CSS Path Resolution

### Standard Path Structure
```
project-root/
├── css/
│   └── style.css
├── img/
│   └── [images]
└── [page].html
```

**CSS link:** `<link rel="stylesheet" href="css/style.css" />`

### Nested Directory Structure
```
project-root/
├── template/
│   ├── css/
│   │   └── style.css
│   ├── img/
│   │   └── [images]
│   └── [page].html
```

**CSS link:** `<link rel="stylesheet" href="css/style.css" />` (relative to HTML file)

## Styling Classes Reference

### Layout Classes
- `.page-container` - Wrapper for article + right sidebar layout
- `article` - Main content container (max-width: 42rem, centered)
- `.content` - Content section wrapper

### Sidebar Classes
- `.floating-sidebar` - Right sidebar container
- `.floating-sidebar-left` - Left sidebar container
- `.sidebar-nav` - Right sidebar navigation
- `.sidebar-nav-left` - Left sidebar navigation

### Typography Classes
- `.date` - Date display (gray, 16px)
- `.comment` - Code comments (gray)
- `.parameter` - Code parameters (red)

### State Classes
- `.active` - Active navigation link

## Responsive Behavior

### Mobile (< 768px)
- Sidebars hidden
- Full-width article
- Padding: 64px 24px

### Tablet (768px - 1023px)
- Sidebars hidden
- Full-width article
- Padding: 64px 0

### Desktop (1024px+)
- Sidebars visible
- Article centered (max-width: 42rem)
- Sidebars positioned 288px from article edges

## Content Guidelines

### Article Structure
1. Start with `<h1>` page title
2. Add date with `<p class="date">`
3. Wrap all content in `<section class="content">`
4. Use semantic heading hierarchy (h1 → h2 → h3)
5. Add bottom margin with `.content` (80px)

### Writing Style
- Use clear, concise language
- Break content into logical sections with h2/h3
- Use code blocks for commands and technical examples
- Include descriptive alt text for all images
- Add IDs to headings for deep linking

## File Naming Conventions

### HTML Files
- Use descriptive names: `ollama-launch.html`, `getting-started.html`
- Use kebab-case for multi-word names
- Keep names concise but meaningful

### Image Files
- Use descriptive names: `ollama-launch-1.png`, `screenshot-example.png`
- Include numbers for sequences: `-1`, `-2`, `-3`
- Use lowercase with hyphens

### Directory Structure
```
project-root/
├── css/
│   └── style.css
├── img/
│   ├── favicon.png
│   └── [page-specific-images].png
├── [page-name].html
└── template/
    ├── css/
    ├── img/
    ├── template.html
    └── sidebar.html
```

## Quick Start Checklist

When creating a new HTML page:

1. ✓ Copy appropriate template (template.html or sidebar.html)
2. ✓ Update `<title>` and meta description
3. ✓ Verify CSS path is correct relative to file location
4. ✓ Add page title in `<h1>`
5. ✓ Add date if applicable
6. ✓ Wrap content in `<section class="content">`
7. ✓ Add IDs to all h2 and h3 elements
8. ✓ Update sidebar navigation links
9. ✓ Mark current page as active in sidebar (if using right sidebar)
10. ✓ Verify all image paths are correct
11. ✓ Test responsive behavior at different breakpoints

## Common Patterns

### Article with Multiple Sections
```html
<article>
    <h1>Main Title</h1>
    <p class="date">March 12, 2026</p>
    
    <section class="content">
        <h2 id="introduction">Introduction</h2>
        <p>Introduction text...</p>
        
        <h2 id="getting-started">Getting Started</h2>
        <p>Getting started content...</p>
        
        <h3 id="prerequisites">Prerequisites</h3>
        <ul>
            <li>Requirement 1</li>
            <li>Requirement 2</li>
        </ul>
        
        <h2 id="examples">Examples</h2>
        <pre><code>example code here</code></pre>
    </section>
</article>
```

### Navigation with Active State
```html
<aside class="floating-sidebar">
    <h3>Pages</h3>
    <nav class="sidebar-nav">
        <a href="page1.html">Page 1</a>
        <a href="page2.html" class="active">Page 2</a>
        <a href="page3.html">Page 3</a>
    </nav>
</aside>
```

## Best Practices

1. **Consistency:** Always use the same template structure
2. **Semantics:** Use proper HTML5 semantic elements
3. **Accessibility:** Include alt text, proper heading hierarchy, and ARIA labels where needed
4. **Performance:** Optimize images before adding to `img/` directory
5. **Maintainability:** Keep sidebar links updated across all pages
6. **Validation:** Ensure all HTML is valid and well-formed
7. **Paths:** Always use relative paths for internal resources
8. **IDs:** Use descriptive, kebab-case IDs for all section headings

## Notes for AI Agents

- The CSS file (`style.css`) is fully self-contained and should not be modified
- All styling is handled through existing classes - no inline styles needed
- Sidebars are automatically hidden on mobile via CSS media queries
- The `.page-container` wrapper is only needed when using the right sidebar
- Left and right sidebars can be used independently or together
- Always verify CSS path relative to the HTML file location
- Images should be placed in an `img/` directory relative to the HTML file
- The template uses a clean, minimal design - avoid adding unnecessary elements
