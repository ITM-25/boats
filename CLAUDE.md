# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This is a static website project documenting Gavin Young's "Slow Boats to China" journey. The site consists of HTML pages that follow a consistent template structure defined in the `/template/` directory. Content is organized in the `/contents/` directory for main pages and `/Notes/` directory for reference materials.

## Common Commands

Since this is a static HTML/CSS project, there are no build steps or dependencies:

- **Viewing pages**: Open any HTML file directly in a web browser
- **Editing**: Modify HTML files in `/contents/` or `/Notes/` directories
- **Styling**: CSS is located in `/template/css/style.css` and should not be modified
- **Images**: Store images in `/contents/img/` or `/template/img/` directories

## Code Architecture and Structure

### Template System
All pages follow templates defined in `/template/`:
- `template.html`: Basic article layout with left sidebar for project navigation
- `sidebar.html`: Article layout with right sidebar for page navigation (wrapped in `.page-container`)

### Required HTML Structure
Every page must include:
```html
<!doctype html>
<html lang="en">
    <head>
        <meta charset="UTF-8" />
        <meta name="viewport" content="width=device-width, initial-scale=1.0" />
        <title>[Page Title] · Slow Boats</title>
        <meta name="description" content="[Page description]" />
        <link rel="icon" href="../img/favicon.png" type="image/png" />
        <link rel="stylesheet" href="css/style.css" />
    </head>
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

        <!-- Left Sidebar (for project navigation) -->
        <aside class="floating-sidebar-left">
            <h3>Projects</h3>
            <nav class="sidebar-nav-left">
                <!-- Navigation links -->
            </nav>
        </aside>
    </body>
</html>
```

### Content Guidelines
1. Start with `<h1>` page title
2. Add date with `<p class="date">`
3. Wrap all content in `<section class="content">`
4. Use semantic heading hierarchy (h1 → h2 → h3)
5. Always include `id` attributes on h2 and h3 for anchor links (kebab-case)
6. Store images in `img/` directory relative to the HTML file
7. Use descriptive alt text for all images

### Navigation Patterns
- **Left Sidebar** (`.floating-sidebar-left`): For project navigation
- **Right Sidebar** (`.floating-sidebar`): For page navigation within a section
- Add `class="active"` to current page link in sidebar

### Styling
- All styling is handled through existing CSS classes in `/template/css/style.css`
- Do not modify the CSS file or add inline styles
- Available utility classes: `.comment` (gray), `.parameter` (red)
- Sidebars are automatically hidden on mobile via CSS media queries

### File Organization
```
/ (root)
├── contents/           # Main HTML pages
│   ├── fernet.html
│   ├── meals.html
│   ├── suitcase.html
│   └── index.html
├── template/           # Templates and shared assets
│   ├── css/
│   │   └── style.css
│   ├── img/
│   └── template.html
├── Notes/              # Reference materials and source content
│   ├── fernet.md
│   ├── suitcase.md
│   ├── meal_list.md
│   └── index.md
└── CLAUDE.md           # This file
```

### Best Practices
1. **Consistency**: Always use the same template structure
2. **Semantics**: Use proper HTML5 semantic elements
3. **Accessibility**: Include alt text, proper heading hierarchy
4. **Performance**: Optimize images before adding to img/ directories
5. **Maintainability**: Keep sidebar links updated across all pages
6. **Validation**: Ensure all HTML is valid and well-formed
7. **Paths**: Always use relative paths for internal resources
8. **IDs**: Use descriptive, kebab-case IDs for all section headings

### Creating New Pages
1. Copy appropriate template (template.html or sidebar.html) from `/template/`
2. Update `<title>` and meta description
3. Verify CSS path is correct relative to file location
4. Add page title in `<h1>`
5. Add date if applicable
6. Wrap content in `<section class="content">`
7. Add IDs to all h2 and h3 elements (kebab-case)
8. Update sidebar navigation links
9. Mark current page as active in sidebar (if using right sidebar)
10. Verify all image paths are correct
11. Test responsive behavior at different breakpoints